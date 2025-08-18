---
title: "A more succint Go Temporal API"
aliases:
- temporal-api
date: 2025-08-17T15:11:47-05:00
lastmod: 2025-08-17T15:11:47-05:00
publishDate: 2025-08-17T15:11:47-05:00
tags: ["Go", "Temporal"]
---

> The code for this post is available on [GitHub](https://github.com/0x5d/temporal-example).

Temporal is a nice tool, providing a useful model for executing distributed processes along with the mechanisms you need to run them reliably, like retries & timeouts.

However the Go SDK can make your code very noisy. In a language where every line is really 3 or 4 lines,

```go
res, err := doStuff()
if err != nil {
    return nil, err
}
```

any amplification of noise is very much unwelcome in my opinion.

That's why I took the opportunity when the need arose at work to start a new component from scratch, and explored alternative designs. Here's the journey and the pattern I arrived at.

## The explosion

Go is a very simple language. Throughout my years writing it, I've learned to keep things simple and explicit.
Let's say we wanna create a function to fetch a URL and unmarshal its response from JSON into a struct. Ideally, our code would look something like the following. I will omit logging & metrics, which would add the same amount of noise across the alternatives we will explore.

```go
func FetchAndMarshal(ctx *context.Context, url string) (*Response, err) {
    rawBytes, err := fetchURL(ctx, url)
    if err != nil {
        return nil, err
    }
    return marshalResponse(rawBytes)
}
```

The caller code would look like so:
```go
res, err = FetchAndMarshal(ctx, url)
if err != nil {
    return err
}
```

Now, let's say we need to turn `FetchAndMarshal` into a Temporal workflow, executing `fetchURL` & `marshalResponse` as activities within it. Let's see what that would (usually) do to the code.

```go
func FetchAndMarshal(ctx workflow.Context, url string) (*activities.Response, error) {
	fetchOpts := workflow.ActivityOptions{
		StartToCloseTimeout: time.Minute,
		RetryPolicy: &temporal.RetryPolicy{
			InitialInterval:    time.Second,
			BackoffCoefficient: 2.0,
			MaximumInterval:    time.Minute,
			MaximumAttempts:    4,
		},
	}
	ctx = workflow.WithActivityOptions(ctx, fetchOpts)

	var responseBody []byte
	err := workflow.ExecuteActivity(ctx, activities.FetchURLName, url).Get(ctx, &responseBody)
	if err != nil {
		return nil, err
	}

	marshalOpts := workflow.ActivityOptions{
		StartToCloseTimeout: 30 * time.Second,
		RetryPolicy: &temporal.RetryPolicy{
			InitialInterval:    500 * time.Millisecond,
			BackoffCoefficient: 1,
			MaximumInterval:    time.Second,
			MaximumAttempts:    3,
		},
	}
	ctx = workflow.WithActivityOptions(ctx, marshalOpts)

	var result *activities.Response
	err = workflow.ExecuteActivity(ctx, activities.MarshalResponseName, responseBody).Get(ctx, &result)
	return result, err
}
```

And the caller...
```go
workflowOptions := client.StartWorkflowOptions{
    ID:        "fetch-and-marshal-workflow-" + time.Now().Format("20060102-150405"),
    TaskQueue: "fetch-and-marshal-queue",
}

var result *activities.Response
we, err := c.ExecuteWorkflow(context.Background(), workflowOptions, workflows.FetchAndMarshalWorkflowName, url)
if err != nil {
    return nil, err
}

return result, we.Get(context.Background(), &result)
```

The increase in lines of code, ignoring empty lines, is more than 3x. Keep in mind, this is a very small example - on a real codebase this could mean hundreds or thousands lines of accidental noise if it's not kept in check.

The only redeeming quality of using the Temporal SDK API directly is its explicitness. You definitely know you're executing a Temporal workflow or activity - not just calling a local function. This signals that you should at least expect some latency, as well as any and all errors that may come from using the network.

You also get flexibility: as you might have seen in the example above, the `ActivityOptions` embedded in the context passed to each activity's corresponding `ExecuteActivity` are different.

That being said, I don't think it's worth it to pollute your code with all that noise, and you usually don't need to change each activity's timeouts & retry policy in the context of a specific workflow (they might be different across workflows, if the activities are reused).

### The alternative

Abstract the noise away.

Every call to `ExecuteWorkflow` and `ExecuteActivity` looks pretty much the same. Define the execution options, embed them in the context, execute the workflow or activity. Furthermore, redefining the execution options every single time (i.e. at every point each activity is executed) is often unnecessary, since they are usually inherent to the nature of the workflow or activity. Therefore, we should be able to define them once.

By defining an interface to get an activity's metadata (name & options), we can refer to it instead of re-declaring the same values each time.

```go
package activities

import "go.temporal.io/sdk/workflow"

type Meta struct {
	Name    string
	Options *workflow.ActivityOptions
}

type ActivityMeta interface {
	Meta() *Meta
}
```

Here's the `ActivityMeta` implementation for the `FetchURL` activity.
```go
type FetchURL struct {/*fields*/}

type FetchURLArgs struct {
	URL string `json:"url"`
}

func (*FetchURL) Meta() *Meta {
	return &Meta{
		Name: "FetchURL",
		Options: &workflow.ActivityOptions{/*Default options*/},
	}
}
```

Notice how we don't need an actual instance of the struct. The function is "static", so to speak (in Go terms: we're using a nil receiver). We can do the same for `MarshalResponse`:

```go
type MarshalResponse struct{}

type MarshalResponseArgs struct {
	ResponseBody []byte `json:"url"`
}

func (*MarshalResponse) Meta() *Meta {
	return &Meta{
		Name: "MarshalResponse",
		Options: &workflow.ActivityOptions{/*Default options*/},
	}
}
```

We can then define a generic `Execute` function and simplify calling these and any activities we define.

```go
package activities

import (
	"go.temporal.io/sdk/workflow"
)

type Option func(o *workflow.ActivityOptions)

func Execute[T any](
	ctx workflow.Context,
	activity ActivityMeta,
	args any,
	opts ...Option,
) (*T, error) {
	meta := activity.Meta()
	if meta.Options == nil {
		meta.Options = &workflow.ActivityOptions{}
	}
	for _, o := range opts {
		o(meta.Options)
	}
	future := workflow.ExecuteActivity(
		workflow.WithActivityOptions(ctx, *meta.Options),
		meta.Name,
		args,
	)
	var res T
	return &res, future.Get(ctx, &res)
}
```

And then our workflow function becomes:
```go
func (*FetchAndMarshal) FetchAndMarshal(ctx workflow.Context, args *FetchAndMarshalArgs) (*activities.Response, error) {
	var fetch *activities.FetchURL
	responseBody, err := activities.Execute[[]byte](ctx, fetch, activities.FetchURLArgs{URL: args.URL})
	if err != nil {
		return nil, err
	}

	var marshal *activities.MarshalResponse
	return activities.Execute[activities.Response](ctx, marshal, activities.MarshalResponseArgs{ResponseBody: *responseBody})
}
```

This is so much nicer, in my opinion. It also restricts flexibility, but not by much - you can modify the `ActivityOptions` by defining and passing one or more `Option` functions to mutate them.

The pattern can also be replicated for workflows and child workflows. See the repo for an [example](https://github.com/0x5d/temporal-example/blob/main/pkg/workflows/start.go).

### Bonus: Self-Registration

Keeping track of which workflow uses which activities is also painful when registration is done throughout the codebase. A saner way to do this is to have each workflow register itself and its activities. For example:

```go
func (fam *FetchAndMarshal) Register(w worker.Worker) {
	w.RegisterWorkflow(fam.FetchAndMarshal)
	w.RegisterActivity(activities.NewFetchURL().FetchURL)
	w.RegisterActivity(activities.NewMarshalResponse().MarshalResponse)
}
```

Registration in the workers becomes as easy as:
```go
	w := worker.New(c, taskQueue, worker.Options{})
	workflows.NewFetchAndMarshal().Register(w)
```

This has the added benefit that it makes it obvious for anyone reading (especially humans) which workflows and activities are used by which parent workflow.
