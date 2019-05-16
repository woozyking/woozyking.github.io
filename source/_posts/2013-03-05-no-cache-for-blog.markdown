---
layout: post
title: "No Cache for Blog"
date: 2013-03-05 21:07
tags: web
---
PHP was the first programming language I learned, no matter how bad you think it is, it has its fair share of contribution toward my knowledge pool today. During that learning process, other than programming insights and techniques, I learned two things:

1. PHP is frustrating to work with
2. Wordpress is a really good blogware

After years of using Wordpress, I decided to try something new. It's not like I blog a lot anyways, but it's nice to have something to perform my *brain-dump*. The major reason behind this decision is due to the way blogware of its generation are flawed by design.

How so?
<!-- more -->
__Blog should not be hidden__. You've decided to start a blog on the web, there should be nothing to hide. So why even store the content in some sort of datastore that's meant to prevent access from unauthorized means? While it's really [not](https://en.wikipedia.org/wiki/Cross-site_scripting) [100%](https://en.wikipedia.org/wiki/SQL_injection) [safe](https://en.wikipedia.org/wiki/Cross-site_request_forgery) anyways. If you do need to hide something, delete it or transfer it to local storage for reference, but there's always a chance that it has been [archived](https://en.wikipedia.org/wiki/Web_archiving) [somewhere](https://archive.org/web/web.php). And you need to *publish* something that's password protected you say? Well, implementing and/or applying such mechanism would just open another can of worms. So really, blog it out loud or keep it offline.

__Blog content [should not be dynamic][1]ally generated__. The way (most) blogware, or blog engine if you will, still has the very concept inherited from one of its [origins](https://en.wikipedia.org/wiki/Bulletin_Board_System). These forms of web applications are meant to be *truly dynamic*, while blogs are mostly static in terms of the content style they represent. You write something on your blog, mostly words and HTML markups, which should all be static by the time it reaches visitors retina. You want to modify it you say? Well, how hard is it to modify a piece of file with plain text than log into your blog admin panel, choose the specific content and then modify the content? Hold on, why don't we *cache* the dynamically generated content and serve that? Sure, that's not a bad technique to speed up blogs. In fact, that's a recommended way of fixing the aforementioned problems. But why cache if you don't have to even generate the content dynamically?

Other personal opinion on the matter:

1. WYSIWYG is so [1974](https://en.wikipedia.org/wiki/WYSIWYG#Historical_notes). Depends on the use case, some times having a [healthy mixture](https://www.sublimetext.com/) from both worlds beats purity. For the content part, markdown (or alike) really serves me well.
2. Wordpress has gone from a lean, agile blogware to a monster. It's getting ever powerful, yet many of those features are derived from the needs of, how do I put it, non-geeky users; and from the threat of Tumblr and alike. Regardless, it is still one of the best open source projects out there, it's just that I don't need those features (any more).
3. Advance third-party, hosted discussion/commenting systems really kick out my last reason of keeping a dynamic system for blogging. I'm putting Disqus down there for grammar nazis and bs detectors.
4. Jekyll. Just because. If for some reason you don't want or can't install Ruby on your machine (or Python for Hyde), try [Farbox](https://www.farbox.com/).

So I'm using Jekyll (actually <del>Jekyll Bootstrap</del>[Octopress](https://octopress.org/)) now, which is not strictly a blogware, but a static content generator that can be used to create a blog. The resulting content is truly static, therefore not as error/attack prone, less things to worry about than the content itself, and faster.

[1]: https://mkaito.github.com/2010/why-jekyll.html
