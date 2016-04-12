# Lifecycle of a Task

We'll follow one task from it's inception as an idea, all the way to it's implementation and the ultimate release to the user.

## 1. From Ideas to Epics

Engineers are usually concerned with the **HOW** should we build something, but when fused together with the rest of the company, especially product managers we're engaged with the question **WHAT** to build.

The answers to WHAT's are ideas and as long as they're not put in writing they stay ideas in someones head that will never reach the stage of actual execution.

----

**Important general note**

One of key principles at Zemanta that touches almost all aspects of our work is:

***"If it's not in writing it doesn't exist!"***

Examples:

* if you didn't send an email about a release, it wasn't released
* if you didn't write an update on trello as a comment, you weren't working on it
* if you didn't send an invite for that meeting, it didn't took place

----

If an idea is not just a small alteration of an existing feature, but introduction of an entirely new concept into our product, then product managers put their idea into writing in a **product requirements document (PRD).**

## 2. Product Requirements document

A PRD is in writing because it makes a product manager's job easier, since they're able to gather all the input that's coming from many channels (i.e. sales, engineering, client's themselves) and then formulate a feature in writing so all the stakeholders that are even separated by continents and time zones can observe and participate in the process of refining ideas into the final feature.

You'll see that a PRD can contain:

* market research notes
* links to correspondence with clients
* feature rationale
* diagrams, wire-frames and whatever it takes to explain the UI to the person who'll be developing this
* ...

## 3. Epic

Once a PRD is synced with stakeholders it will go to a **milestones** trello board, it will be placed in on trello ticket and we call that an **epic**.

The milestones trello board is in place so everybody has a clear overview of the road-map on a single dashboard.

## 4. Backlog

On a weekly basis, it's both a product manager's (PM) and tech lead's (TL) job to negotiate the priority of execution of epics and once that's determined both the PM and TL will transfer epics onto a **backlog trello board**.

PM & TL will also look into the size of an epic and they might break it down to smaller deliverable tickets on the backlog.

Once tickets end up on the backlog it's also time for engineers to get involved by providing technical feedback back to PM - in writing of course. We call this technical feedback **tech notes**.

The purpose of providing tech notes on the backlog is to provide a brief outline how a feature will be implemented, obtain additional clarification from PM and to answer common questions a PM might have:

* what's the basic outline of the implementation?
* which systems will the new feature alter?
* will this feature yield major architectural changes?
* what's a ballpark estimate on how long will it take to implement and how many of your team mates will this occupy?
* what are the known unknowns going forward?
* where are the risks of this getting really complicated which would delay the expected deliver?

Do note that tech notes are by no means full blown technical documentation, so keep things short and to the point.

## 5. Main Board

Once all prerequisites are completed on a backlog based ticket:

* presence of a PRD
* tech notes
* having a PM and at least one engineer responsible assigned

.. than we can proceed with the actual implementation by placing the ticket on the **main board**.

The main board has 5 columns:

1. **Accepted** - on a weekly meeting with your team, you'll negotiate and agree on what will you commit to executing in the short term, you're able to do so by placing a ticket in this columns
2. **Doing** - shortly after you being, you can let your team know you're working on an item by placing it in this column
3. **Blocked** - if you encounter anything during the implementation that's blocking you to move forward, let your team know by placing it in this column *(e.g. the 3rd party API that was provided on the PRD is no longer supported and throws 500 on every call - we need to discuss on how to move forward)*
4. **Review & Testing** - once you've finalized your implementation and are waiting for feedback from PMs or fellow engineers for a code review feedback, you can update the status of the item you're working on by placing it under this column
5. **Done** - once you hear back from PMs, your peers approve of your code in the code review process and you've successfully deployed your code into production it's time to wrap things up and place the item under this column

On your weekly team meeting you'll also validate the completion and archive the trello ticket placed in the done column.

Do note that every engineering team has their own backlog and their main board, but we have a single milestones board.

## 6. Release

At Zemanta we release stuff in 2 distinct ways:

* **Release with deploy** - if a feature doesn't directly affect our end users (e.g. we deploy a minor backend bidding algorithm adjustment) we deploy and that also counts as a release. Once you deploy it's important that you update your team via email and or trello to let them know something is released.

* **Release with feature flag** - however, if a feature does directly impact our end users (e.g. new button in a dashboard) we have a very intricate internal mechanism in place that allows a developer to hide their feature behind a feature flag, making the feature to be only available to a subset of users. Since PMs have the control to assign feature flags to subsets of users, they can release a feature whenever they please, relieving engineers of the responsibility of an release.

Talk to your tech lead to get familiar on how to use our internal feature flags mechanism.
