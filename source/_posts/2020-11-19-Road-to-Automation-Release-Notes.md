---
title: "Road to Automation: Release Notes"
date: 2020-11-19 21:48:12
tags:
  - programming
  - software development
  - automation
---

Automation is not a binary thing. We rarely go from manual to fully autonomous with one switch. Just like software development itself, it often requires iterative revisions, with some craftsmanship and never-enough imaginations.

A case I would love to share is our 8-month and counting journey we took to automate our release notes process.
<!--more-->
Writing release notes for the software we proudly make isn't always easy. Even some of the most popular applications don't get the release notes they deserve (and please don't learn from them).

![where to be found?](https://i.imgur.com/LFlaQvU.png)

Fortunately, we can also get inspiration from some **great examples**.

![hybrid](https://i.imgur.com/D4JcCmP.png)

![story](https://i.imgur.com/AbJB9iF.png)

Having concise and coherent release notes sure are great, but they often require a lot of effort and time that most developers don't have.

Thus we [initiated a project](https://github.com/EQWorks/release) to help ourselves to relatively effortlessly produce release notes or at least the materials that save time for human editorial.

We started at the source of software changes -- commits (at the time of writing, this refers to git commits). We laid out [some conventions](https://github.com/EQWorks/common/blob/master/git.md#on-commits) to encourage good commit and commit message practices. The adaptation of these conventions alone allows our developers to think more on behalf of the end-users, something akin to the [concept of having everyone bearing the responsibility of marketing](https://basecamp.com/handbook/01-basecamp-is-you). And for our objective of automation, this gives us a solid starting point due to having a predictable structure of our commit messages.

The next step was to prototype the core capabilities of parsing out commit messages and constructing a markdown formatted release notes. Given our existing practice of utilizing git tags for releases, we quickly made the first usable version.

The first version would require [a configuration file](https://github.com/EQWorks/release/blob/de8bff3b83c28ed54cef80ab9a5a31129cceffd5/.env.example) to specify the target project git repository and which tags to compare. Both are apparent areas to improve on. So we progressed it to be a [portable CLI with automatic tag discovery](https://github.com/EQWorks/release/commit/310483172b060051f729c4ceffb3501dcac3f314) that works in any git repository.

The portable CLI nature and its usage simplicity allowed the team to adopt the tool and organically formed a process to perform releases with this tool being a part of it:

1. Create a git tag (at the time of writing, we usually do this through GitHub releases).
2. Sync local repo to obtain the created git tag.
3. Run the `release notes` command to generate release notes.
4. Copy/paste the output into where we retain the release notes (again, usually the GitHub release from step 1).

The (manual) process highlighted opportunities for further automation. We introduced [an option](https://github.com/EQWorks/release/commit/21d627883c2d5ff680c61f54ff429629ec1a581f) to directly update the GitHub release notes, which eradicated step 4 from above. Then we started integrating the release CLI as [a part of the CI/CD (continuous integration/deployment) workflows](https://github.com/EQWorks/notify/blob/master/.github/workflows/npm.yml#L40) of our growing number of projects, which eliminated the need for steps 2 and 3. And at the time of writing, we're attempting to [automate away step 1](https://github.com/EQWorks/release/pull/24) as well.

At this point, we'd have an established adoption of the tool, with little to no accessibility inhibition and minimal human involvement to initiate a release. So after about six months of active usage, we took our next step into refining its core capabilities. We added an alternative output formatting that we adapted from [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), as well as an NLP (Natural Language Processing) model [for labeling](https://github.com/EQWorks/release/pull/14) the nature of each commit message.

We initially adopted the NLP model from [a random but lucky encounter](https://github.com/gesteves91/fasttext-commit-classification) and then [revised it](https://github.com/EQWorks/release/pull/20) using the [same tool](https://fasttext.cc/) made available from Facebook Research. The model is far from perfect, and its training dataset is still lacking. We already have some ideas lined up [to seek a better model](https://github.com/EQWorks/release/issues/25) **automatically**.

Today, for some software, such as libraries and tools intended for other developers, we think it's almost sufficient to use the release tool we made to generate their release notes, such as you can see at [release's releases](https://github.com/EQWorks/release/releases).

But the ones for our end-users, we would still take some time to grey out irrelevant technical details or summarize out the noteworthy highlights based on the generated release notes.

![grey out](https://i.imgur.com/eyFl3SG.png)

![highlights](https://i.imgur.com/fhYFtL4.png)

The still needed human involvements are perhaps the next targets to improve on, and the [increasingly available](https://openai.com/blog/openai-api/) and [highly accessible](https://pytorch.org/) tools at our disposal will only make it easier by the day.

The above was a piece of our journey, but if you can identify manual chores among any tasks you do, you may hit on this adventurous road of automation too. It shall be rewarding.
