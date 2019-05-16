title: "Open Source Charting Libraries I Have Tried"
date: 2015-03-14 01:22:28
tags: ['open source', 'data visualization', 'charting', 'SVG', 'Canvas']
---
In a recent project, I needed to maintain quite a number (8-9 and growing) of charts that support "real-time" data updates (1 second interval) on a single page. The following open source libraries are what I have tried.
<!-- more -->
#### [`C3.js`](https://c3js.org) - Based on d3.js, SVG

What C3.js stood out to me was its superb documentation - straight to the point code examples, combined with concise API references. I never wasted a minute to find what I needed was there, or not. Unfortunately its performance suffered greatly from the number of charts I needed to have on a single page, and the frequency of data points being updated.

It is however very suitable for use cases that do not require real-time updates.

#### [`Chart.js`](https://www.chartjs.org) - Canvas

I moved on and picked this non-SVG based library with the most stars on GitHub. It has great documentation, rich and extensible APIs, and plenty of chart types. However I only tried it for a brief moment and gave it up for a few simple reasons. You have to configure colors for each dataset, sometimes more than one per dataset depending on chart types, and you cannot even style it with CSS. In fact, the whole library is CSS free (which is not necessarily a bad thing, and logical for a pure Canvas based solution). And there is no emphasis and mention about its real-time capabilities. The given APIs definitely does not look like it is designed for such use case.

#### [`Epoch`](https://fastly.github.io/epoch) - Based on d3.js, SVG for Basic Charts, SVG + Canvas for Real-time Charts

During the library hunt, I came across [this answer on stackoverflow](https://stackoverflow.com/a/12335448/158111) regarding performance issues with pure SVG implementations and how to resolve them. That gave me the direction to land on the next one - Epoch. It applies the technique of utilizing partial Canvas to optimize real-time charts rendering, just what I need. One downside of using it is that its styling can be tedious, especially without the use of a CSS preprocessor.

Epoch is great for use cases where you need to setup real-time monitors for things like CPU usage, live trend of tweets count on certain hash-tags and so on. It is __not__ for the type of data visualization that encourages user to interact with. It does not have any type of interactivity support, not even for its Basic Charts as of version 0.6.

#### [`cubism`](https://square.github.io/cubism) - Based on d3.js, SVG + Canvas

Another popular library that applies similar technique as Epoch, probably more advanced for customization. Unfortunately, the documentation is too much for me to digest quickly, and not to the point (no live code examples, mostly).

#### Other Notes

Keep in mind that all the pros and cons of aforementioned libraries are greatly biased by a single use case, plus my personal preference of documentation style and API friendliness. Ultimately I would just tailor a solution based on [d3.js](https://d3js.org/) for maximum flexibility and control. I think I am going to learn it, hopefully soon. For now, I have settled with [`Epoch`](https://fastly.github.io/epoch), because it is data visualization dummy friendly,
