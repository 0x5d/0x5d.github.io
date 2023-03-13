This template is a companion for my [Writing RFCs](https://0x5d.github.io/posts/rfcs/) post. This is meant to be adapted to your specific needs. If something doesn't work for you, get rid of it or change it!

You should also look at other RFC templates, such as [Hashicorp's](https://works.hashicorp.com/articles/rfc-template).

# RFC: (Title here)

Authors: (Your name goes here)
Date:

## Abstract
A brief, 1-paragraph description of the problem and the solution. This will help readers contextualize themselves before diving into the RFC. 

## Background
What additional context would be valuable for the reader? Write it here! It can be historical (e.g. our user-base has increased one order of magnitude in the past year) or technical.

### Motivation
What's the main motivation to tackle this problem? What are the main benefits?

## Requirements
These help set the limits on the solution space, making it easirer to motivate your proposal (next section). They can be functional and non-functional.

## Proposal

### Summary
This is a high level description of your proposal. After reading it, the reviewers should have a well formed picture of its direction, scope and expected results.

Architecture and flow diagrams, wireframes and prototypes work really well here, as they help readers visualize the solution from different angles.

### Detailed design
This is where you go into the weeds, listing all the finer-grained changes that your solution requires. Don't be afraid to be precise and specific - the previous sections were about showing a bird's eye view of the puzzle, but this is where you show what the pieces are and how they fit together.

### Operations
The purpose of this section is to answer questions like "how will it be deployed?", "how will the migration look like?". Consider scenarios in your new solution's licecycle, such as updates, failover, scenarios where manual intervention is needed, etc. 

### Metrics
List and describe all the metrics that will need to be monitored to check that the new feature, component, etc., is working well.

### Costs
Nothing is free! What is the cost of implementing your proposal?

### Risks
Go over any factors which could put your plan in danger, as well as any mitigation strategies.

## Alternatives

This is the section where you list the alternatives you considered. The 1st alternative should always "do nothing", and it's like the complement to the Motivation section. While "Motivation" is about exploring the ideal future, "do nothing" is about exploring the consequences of not changing anything.

Normally, there should be 1 or 2 alternatives other than "do nothing". If you don't have them yet, it might be a sign that you need to explore the solution space further.

## References

List any external material you used, so reviewers can cross-check.

## Apendices

Any supplementary information or explanations may go here. This is usually info that might be useful for a small subset of your audience, something to add "just in case".
