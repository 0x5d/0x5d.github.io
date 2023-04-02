---
title: "On writing RFCs"
date: 2023-02-18T15:11:47-05:00
lastmod: 2023-03-12T15:33:47-05:00
publishDate: 2023-03-12T15:33:47-05:00
tags: ["design"]

ShowReadingTime: true
---

I'll write the post I wish I'd read before I started writing RFCs at work. I'll start by providing some motivation for RFCs, then go over my _current_ process for writing them and provide a [template] that you can use.

This post isn't by any means a formal specification for writing RFCs. If anything, it's a loose framework I've found helpful over the past couple of years. I'm also writing this as a reference for myself in the future, but I hope it'll be valuable for you too. It took me a while to appreciate the value of RFCs, but now I consider them one of the helpful tools I use regularly. If you're an RFCs veteran, maybe this will give you a different perspective on them, and if you have tips, please share them! If you've never written an RFC, I hope when you're finished reading this post you'll feel confident enough to invest the time in one next time you get the chance.

**Hold on - what's an RFC?**
RFC stands for "Request for Comments". It's a structured document written in natural language explaining a proposal. Most of the time, RFCs are written for technical proposals. Still, they could be used to propose a change in an organizational process.

## Why?

I've found 3 main benefits of writing RFCs. They are each related to each general stage of the decision-making process: _before_, _during_ and _after_. In short, I like that they facilitate an **upfront analysis** of the change and its implications, encourage **asynchronous collaboration** and thoughtful discussion, and serve as a **future reference** to understand how and why the decision was made.

In a healthy organization, this should also mean you'll spend less time overall than if you had just dove right into coding. It often seems counter-productive, but after investing time into writing a good RFC, the path forward should be clearer and smoother in many ways.

### Before: Upfront analysis

Writing an RFC requires thoroughly exploring the problem and solution spaces. The problem might be something you discovered (e.g., a frequently-run query's performance has degraded) or something given to you (e.g., a use-case discovered by the Product or Design team that the product should support). Nevertheless, the problem itself and the requirements stemming from it should be thoughtfully broken down.

This problem description and list of functional and non-functional requirements will be helpful when pondering the different alternatives to solve the problem. They effectively restrict the solution space, guiding you to the optimal - which might not be the "ideal" - solution.

At this point, it's essential to identify co-authors, collaborators, stakeholders, and your audience.
- Co-authors will actively work on writing the RFC with you, committing time to think about it or parts of it.
- Collaborators are people who can guide you or help you bounce ideas off. For example, a team member who owns a subsystem your code will need to interact with, or a product team member who can help you understand the underlying use cases better.
- Stakeholders are any folks impacted by the RFC's implementation. It's a naturally "open" group that might include contributors who must prioritize new work, their managers, customers waiting for a specific feature, etc.
- Your audience is whoever you intend to get feedback from. You must identify it early as it will determine the style of writing you should use and the type of additional material you should include (such as diagrams, code snippets, or wireframes). If you fail to establish your audience, receiving high-quality feedback on your RFC will become more challenging. A clear sign of this is that commenters will usually ask you to explain something better or add more context. In those cases, your reaction shouldn't be to exclude them from the review process - instead, try to understand their background and adapt your RFC to make it more digestible.

I've found the things I described in this section to help reduce the risk (both in magnitude and type) associated with your proposal.

The first risk dimension is resource utilization. If your RFC is approved, you'll better understand **what** needs to be done, **who** should do it, and **how long** it will take. On the other hand, if it's rejected, you won't have wasted your valuable time working on a potentially-complex project only to find out it wasn't needed. Arguably, our work as programmers isn't to write as much code as possible but to minimize the amount of code that needs to be written. 

The second is change management. Your proposal will inevitably affect the system it modifies. Thinking about the problem and solution in such a profound way will help you see how implementing your RFC will impact other components and users, making coming up with a migration strategy an easier task.

The third is the impact on people. If you're writing an RFC, chances are the problem you're tackling isn't trivial. As stated above, it will probably require immediate changes in other components or systems and a timeline for rolling them out (migration). This means you'll need to get the people responsible for those components to approve it. An RFC provides a great space to collaborate with them until everyone is onboard. If they have doubts, you can add more context to clarify everything. Suppose they spot a flaw in your understanding of how other components work. In that case, you can reassess and develop a better alternative.
When your RFC has been approved by all the relevant people, they'll have a clear picture of their role (or their teams' role) and plan accordingly.

### During: (Asynchronous) collaboration

Some RFCs are simple - everyone knows the problem, and the solution is small in scope with a clear execution plan. Some are complex and require several people to write and review. In those cases, getting everyone on a video call is a sub-optimal way to get everyone's thoughts. The most efficient way to write and review RFCs is with tools that enable asynchronous communication.

When you're collaborating with someone on authoring the document, it allows each of you to write and contribute when you're the most productive. Furthermore, writing RFCs usually requires stepping away, drawing diagrams, reading existing code, and sketching solutions.

For the reviewers, it gives them time to read the whole document, take notes and think of insightful suggestions or meaningful questions. This is way more effective than forcing someone to provide an opinion on the spot. As an author, it also helps to digest the comments on your RFC, to prevent misunderstandings or spam. If you or someone uses the comments section of a shared document as a rapid-fire chat, people will tune off and ignore the conversation altogether.

Additionally, RFCs decouple definition from execution. The authors don't necessarily need to be the ones to implement the solution described in it, reducing the bus factor in the team. Consider the alternative, where no RFC was written, and the person in charge switches teams or leaves the company altogether. Reading (sometimes, "deciphering") the code they left behind is usually more time-consuming.

### After: Future reference

After the RFC has been implemented, it's still an invaluable resource, as people can refer back to it if they have questions about _why_ things were done the way they were done. Sometimes, even the fundamental question of _why_ they were done, regardless of the specific approach taken, isn't obvious at all.

In such a dynamic (to put it lightly) industry, people **will** switch teams, and they **will** leave companies. And so will you, probably! Coming into a new team and having a set of well-written RFCs for the most relevant parts is a godsend.

Another minor - but still significant - side-effect of this is making it easier for folks who joined the team recently to empathize with employees with a longer tenure. It can be easy to judge someone harshly when you see a part of the codebase they contributed that doesn't make sense. "Bad" code often can't be taken at face value, and RFCs help with that. If the requirements were documented, they could say a lot about the context in which it was written and the constraints that limited the implementation. 

## When?

In time, you'll instinctively know when to invest time into writing RFCs. However, a good rule of thumb is to review each of the above reasons for writing one (upfront analysis, collaboration, future reference) and consider whether any will be valuable to you (or future you!). If at least one of them seems like a good thing to have, chances are it'll be time well spent. It's easy to dismiss writing RFCs if your team is small and everyone is on the same page about what needs to be done. However, you'll be surprised at how often your assumptions about how something works - or how it can be changed - turn out to be wrong. When that happens, it's usually way better if you're just writing a document instead of at the last commit of a colossal pull request.

Similarly, teams grow and change, and we forget things. If your company is small now, it doesn't mean it will remain so forever. If it starts growing, you might need to hire people at an increasing rate, and RFCs will help them hit the ground running when they join. Also, when someone new asks you, "what were you thinking when you added this feature?!", you can point them to the RFC and not worry about having to keep every detail in your mind.

Something to remember, though, is that not every change needs an RFC. Some problems are small and self-contained, and their solution obvious or not controversial. In those cases, clear commit messages and a comprehensive cover letter for your pull request will go a long way in documenting the change. When someone wants to learn more about a particular file, they can use `git blame` and read them.

There are no mathematical, completely objective rules to know when or not to invest your time in an RFC. If in doubt, talk to your team and understand their expectations.

## How?

When it's time to get to work, I've used a loose strategy based on the above. 
1. Assess the problem and solution spaces - is an RFC needed?
2. Identify co-authors, collaborators, stakeholders, and audience.
3. Start writing! Take a look at the [template].
4. Share it and get feedback.
5. When all feedback has been addressed, ensure your proposal is generally approved.
6. Implement.

Let me expand on 4. & 5. When sharing an early draft, you'll get a lot of feedback on the _form_, like a note you forgot to remove or requests to add visual material, expand on an idea, etc. If many people review it simultaneously at that stage, you'll get a lot of similar comments. If readers perceive your RFC as a very "dirty" draft, they might not read it all (or at all). The idea is to refine these early on with a group of people that care a lot about the RFC, like your team or your manager. With those comments out of the way, you can expand the group of reviewers. The new round of comments should hopefully focus more on the content.

With regards to getting an RFC approved, it depends on the implicit or explicit agreements or policies within your organization. Some teams require sign-offs (e.g., reviewers mark the document as approved), while others may interpret the lack of outstanding feedback as approval. It's all about communicating expectations effectively.

## Closing remarks

Lastly, keeping an open mind is an essential part of the process. It's a **Request** for Comments. You're actively asking other people to review and critique your proposal. Some folks will like it a lot, while others might not. Even if the latter scenario doesn't feel great, I've found that it's usually the most helpful, assuming good faith and that no one's gatekeeping anything (if you suspect that's not the case, you might have a big culture problem on your hands!). 

If someone isn't convinced with your RFC, it could be that you haven't considered all the alternatives. Sometimes, someone will reveal a major flaw in your RFC, causing its rejection. This might feel disheartening, but it's actually a great outcome! That person saved you from wasting time pursuing an endeavor that wouldn't have ended well. Go back to the drawing board, and try to get them involved so you can come up with an airtight solution.

[template]: https://github.com/0x5d/0x5d.github.io/blob/main/content/posts/rfcs/files/rfc-template.md
