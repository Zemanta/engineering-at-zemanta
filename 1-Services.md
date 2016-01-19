# Services

At Zemanta we rely on multiple 3rd party SaaS services to bare different responsibilities. A prerequisite for new hires, to get a grasp on how we operate, is to first obtain access to all of these services.

All services mentioned below are managed by an **IT manager** - not an full time role at the moment, so VP of Engineering is currenlty in charge. 


## 1. Google apps

Everybody get's a `name.lastname@zemanta.com` email account hosted on google apps. IT manager can provide you one. Not only you'll be able to access your email, calendar, google drive, hangouts etc, but it will also serve you as the prefered method of authenticating with other services i.e. trello, slack, etc.

Within google apps, we use the following products besides email of course:

* **calendar** - in there you'll find meetings that you're required to attend to on a weekly basis, it's your manager's role to make sure you're invited to these meetings where appropriate
* **hangouts** - preferred means for video conferences
* **google drive** - we use google drive very extensively and there you'll be able to find: prouct requirements documents, ad hoc spreadsheets, metting logs and notes, even notes from one on one chats

One more note about Google apps: **make sure you have [2FA](https://www.google.com/landing/2step/) set up**.


## 2. Slack

Disruptive enterprise products like Slack are shifting the main means of communication in companies from email to slack. We were early adopters of slack and it changed how we communicate. You'll find slack being used for:

* quick syncs between teams
* developer and product FYI announcements
* one place to aggregate all pull request comments from github (enabled by one of many [slack's integrations](https://slack.com/apps))
* place to share news and cool finds
* we even have a bot that tells you what our faviourite place serves for lunch today

A prefereed setup for every team member is to have slack set up locally on your machine in a way that **enables you to recieve desktop notifications** while you're in the office. Having slack set up on your phone is neat too.


## 3. Trello

Trello is our main ticket, epic, issue, bug, roadmap tracking service used across all of our teams. We'll get into the details of the actual program management and execution later on, but at this point it's important to note that everbody on zemanta's product and engineering team should treat trello as the main means of communication to track and update on progress of their work. Remember: **if it's not on trello, we don't know you're working on it**.

* If you're an engineer: you manager should make sure you have access and are aware of at least 3 trello boards: your team's main board and backlog boards and the cross-team milestones board managed by product managers.

* If you're a product manager: your manager has a role to provide you with access to appropriate engineering boards and product boards.

## 4. Github

Besides being the main service that hosts all our code, github is also used as a preferred method to authenticate to other engineering services that enable github based OAuth. No need for you to create a separate account, just use your existing github account and **make sure you have [2FA](https://help.github.com/articles/about-two-factor-authentication/)** set up once you're added to the [github.com/Zemanta](https://github.com/Zemanta) organisation. 

Do note that there are many repositories in our arsenal, but the good news is that you'll only have to worry about a handful of them. The rest are considered either legacy, experimental or hackday produced code.

As mentioned above we use github for:

* hosting our git repos
* authenticating to other services (e.g. Circle CI)
* code reviews via pull requests
* we don't do issue tracking via github
* we don't do releases
* we don't do private forks, just push branches directly to our original remotes and base your pull requests off of them


## 5. Circle CI

For every `git push` we run a build on Circle CI, the status of the build is propagated back to github via [status checks](https://github.com/blog/1935-see-results-from-all-pull-request-status-checks) on every pull request (i.e. this let's the code reviewer know that the pull request breaks the build). 

A successful build on `master` branch is also a prerequisite for most of our deployment procedures. 

You can access Circle CI via github OAuth.
















