---
layout: post
title: "Tame (Py)cURL"
date: 2013-07-02 21:19
tags: [python, curl]
---
Recently I needed to have something that allows me to have more control over my data stream client, so that:

1. When desired, it would stop and close the connection as gracefully as possible.
2. A precise counter for how many records have been received is in place.

The initial implementation was quite straightforward:
<!-- more -->
{% gist 5914731 v1.py %}

Though practical, this approach could be improved much further, in particular:

1. Lacks the capability to flip the switch on and off on demand.
2. The number of records needs to be saved "manually", and only at the end of the session (excluding the possibility of nasty interpolation). Something that's accessible mid-session is much more preferable.

Coming up next, we'll tame (Py)cURL further A_A
