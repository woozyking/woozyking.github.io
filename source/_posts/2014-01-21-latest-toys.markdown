title: Latest Toys
date: 2014-01-21 22:02:10
tags: [python, redis, mongodb]
---
I have been toying around with a few ideas lately, the process allowed me to tackle some interesting problems by ways that are not so conventional, at least not the ones from Google page 1's.
<!-- more -->
## Problem One - Random Access of MongoDB Documents

This problem is definitely not new, indicated by the number of times the same or very similar questions asked on Stack Overflow. In general, there are 3 solutions:

1. Add a new field to be filled with a comparable random value, usually a float; index this field for production ready performance.
2. Leverage `skip`, but it is said to be highly expensive.
3. Not exactly a solution just yet, but wait for MongoDB core team to implement a native, server side solution. It is noteworthy that [the issue on their JIRA](https://jira.mongodb.org/browse/SERVER-533) was once closed as "Won't Fix" but eventually re-opened due to popular demands.

I am fairly certain that the server side solution will be as solid as other APIs MongoDB generally exhibits, but it is not here yet. So crossing out _2_ and _3_, and I do not really like _1_. The project is still very young (young enough that I am not talking about its name at the moment), so I can easily `drop` the collection or the entire db in MongoDB to rebuild the structure without worrying about consequences. However, I do not like the fact that one additional field has to be added just to support one use case, especially when it also requires indexing, which could result in a mess if I ever decide to drop it.

So here is my solution:

{% gist 8534651 cache_mongo_ids.py %}

Basically:

* No extra field occupying both disk and RAM (if indexed), delegated to a Redis `SET`
* No messy random ID determination logic in client code, delegated to Redis `SRANDMEMBER` command

Of course, I will likely switch to the official solution when the time comes. But now, I am more than satisfied with what I came up with.

## Problem Two - More Accessible Event/Error Logging in Python

Let me elaborate more:

* I want to be able to get a sense of urgence when error occurs, but not to make myself panic by browsing through 10 gzipped logs containing the same stack trace
* I want to be able to have a central hub to see all logs, but without worrying too much about system resource consumption, such as disk space and RAM

In addition, I have planned to seperate the Redis `Queue` implementations from [`tidehunter`](https://pypi.python.org/pypi/tidehunter) for a while already. Therefore, [`techies`](https://pypi.python.org/pypi/techies) was born. You can also check it out on its [GitHub repository page](https://github.com/woozyking/techies).

To solve problem two, I would do:

```python
from techies import CountQueue, QueueHandler, REF_LOG_FORMAT
import logging

q = CountQueue('app_logs')
q.clear()

logger = logging.getLogger(__name__)
handler = QueueHandler(q)

handler.setFormatter(logging.Formatter(REF_LOG_FORMAT))
logger.addHandler(handler)

for i in xrange(5000000):
    try:
        1 / 0
    except ZeroDivisionError as e:
        logger.exception(e)

print(len(q))  # 1
print(q.get())  # ('ERROR_MSG', 5000000)
```

So imagine that your nicely crafted web app suddenly got hit by hackernews viewers. 5,000,000 views on a particular buggy route resulted in 5,000,000 identical exceptions raised. No sweat, you'll only get one error log, with a clear indication of how many times it was recorded. You saved resource to store logs, and time to dig logs out. Before they even realized, you have deployed the fix already.

As usual, [Redis](https://redis.io/) required.
