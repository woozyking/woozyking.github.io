---
layout: post
title: "Needed a Drop"
date: 2013-05-09 11:24
tags: vps
---
GitHub page has served me well. But since I already signed up a $5 VPS at [digitalocean.com](https://www.digitalocean.com/?refcode=a88edf242e1d), I figured it's time to switch to gain more control and pollute GitHub commit number no more.

Here's a list of things I did with this little devil:
<!-- more -->
1. Signed up their service with a promo code. You'd have to use a credit card to take advantage of that $10 credit, though I really wanted to use my "For fun" budget set aside on Paypal.
2. Spun up a $5 instance within 60 seconds (as they advertised), with Ubuntu 12.10 x64.
3. Added my SSH public keys with their control panel.
4. Logged in with root and the password they sent through email.
5. Changed root password (`passwd`).
6. `apt-get update` and `apt-get upgrade`, without upgrading the kernel.
7. Set up OpenVPN server for Netflix US exclusive shows and China trip.
8. Set up Nginx with virtual host profiles, which includes this very site.
9. Switched A Record from GitHub page to this server IP.
10. Reconfigured Octopress Rakefile to deploy using rsync that through SSH to this VPS.
11. Set up Dropbox on VPS for cheap and selective backup. I don't need to backup the whole server, just some key configurations for OpenVPN server, MySQL dumps (for future Wordpress host), etc. In short, I'm too cheap to pay for the backup and snapshot service they provide.

The only part I struggled a bit was the OpenVPN setup, which took me 2 hours last night but the knowledge gain was impressive.

Next, I want to migrate [xiangpi.ca](https://xiangpi.ca/) to this VPS and say bye to Dreamhost's virtual host, which not only costs more annually, but also too restrictive for my use cases.

I need some thoughts on how to optimize Wordpress on a VPS using APC, Varnish, Memcached, Redis, etc.
