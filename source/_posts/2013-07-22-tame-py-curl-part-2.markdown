---
layout: post
title: "Tame (Py)cURL Part 2"
date: 2013-07-22 12:15
tags: [python, curl, redis]
---
[Previously](/2013/07/02/tame-py-curl/), we discussed the possibility to *hack* PycURL to achieve somewhat a more controllable HTTP streaming client.

Today, let's add some `Redis` ingredients into our recipe and make this tool more controllable so that we can solve the remaining problems aforementioned.
<!-- more -->
The rational behind utilizing `Redis` is that we want a scalable (both server and client), and easy-to-use in memory key-value store to maximize our control over `PycURL`. With that in mind, I actually encouraged the company that I work for to open source this project I created during my day time job.

Introducing `TideHunter`:

-   Accurate quota limit - total control over your stream quota.
-   An instant off switch - when sht hits the fan and you don't want to crash your process.
-   Redis backed control tools - semi-persisted, fast, and scalable.
-   Core mechanisms based on the solid cURL and PycURL - inherits the built-in goodness (gzip support and more).
-   OAuth support based on python-oauth2 - see [this demo](https://github.com/amoa/tidehunter/blob/master/demo/five_tweets.py) in action.

This project is submitted to [PyPI](https://pypi.python.org/pypi/tidehunter), which means that its installation is as easy as:

```
$ pip install tidehunter
```

The repository is on [GitHub](https://github.com/amoa/tidehunter) with sufficient examples to get you started.
