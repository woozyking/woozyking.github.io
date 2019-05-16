---
title: Serverless and Attention Span
date: 2018-04-09 08:40:04
tags: [serverless, programming]
---

Lately I’ve been leveraging the "serverless" architecture often. I like its statelessness, the predictable cost factor, the simplicity of development and deployment, etc. It just fits for systems that are event-driven. It's already a strong tool that is being used by many live products, yet still steadily being improved by providers and the community on aspects such as cold-start time and time-to-live. Not to mention frameworks such as [`serverless`](https://serverless.com) and [`apex`](https://apex.run/) further spoil us with ease of utilization.

There are also many technical reasons one can use to argue against using this architecture. Job security for certain overly-specific professions aside, at the end of the day, it should depend on use cases and automation requirements.

However, one of my rationale to justify the use of this architecture, as mentioned in the title, is related to attention span.

In today's world, it is increasingly hard to be exempted from having a short attention span, which is often caused by constant interruptions of thought process and work-flow.

The interruptions may come from your co-workers that don't understand why it's not optimal to tap on your shoulder while you are in the "zone".

The interruptions may come from your S.O. or close family member who brings up some important matter in your life while you're trying to organize your thoughts on a nested for-loop.

The interruptions may even come from your own self-distractions -- social networks, exciting announcements, or simply an accidental overhearing of topics that can suddenly pull you out your chair, so you can eagerly express your expert opinion.

Unless your work, your life, and yourself can all discipline and behave, having a short attention span is almost unavoidable.

There are rare developers who have adapted or been blessed to have a large enough reservoir of memory, and nimble enough brain power to handle quick and short context switches. If you are, or work with, one of these, congratulations! Some call them "10x" developers, I believe they're truly genius.

Unfortunately, the vast majority of us aren't.

![interruption](/images/memes/jason_heeris_2013_interruption.jpeg)

So here comes the beauty of working with the serverless architecture. By its design and its common limits, the software you deploy serverlessly are hopefully simple and small. That's why providers name their direct serverless offering as "Lambda", or "Functions", and encourage developers to write pure, stateless, stable input-output functions that often focus on one thing at a time. That naturally leads to simpler and smaller of the program that needs to be written (don't be [too extreme](https://github.com/kelseyhightower/nocode) though). And that means shorter tasks that fit better in our shorter attention spans.

Now, before I'm accused of being a sales person of certain provider, or a fan-boy of the architecture, let me lay it out for you what's in it for the providers -- smaller pieces `=` more services needed to solve real-world problems `=` more usage fees $$$. Simple.

So to avoid being ripped off by naively hopping on the hype-train, a new challenge for developers who work with serverless architecture is to figure out a good balance for each service to maximize processing done per billable resource, while keep the logic simple enough or just stateless. That's not always easy, because it often requires assessment of other services that it connects with. In the older times (or larger organizations), this type of duty falls on the hand of dedicated architects, which is a profession that's increasingly obsolete in the younger and smaller organizations (and rightfully so).

"Serverless" is far from being the silver-bullet answer to all problems, it still requires fine tinkering and craftsmanship.

But my point stands, that in a world full of distractions, the naturally simpler, smaller, and often stateless functions help greatly in solution composition, as well as mitigating interruptions coming from whatever angle.
