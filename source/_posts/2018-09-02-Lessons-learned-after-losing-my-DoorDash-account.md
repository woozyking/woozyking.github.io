title: Lessons learned after losing my DoorDash account
tags:
  - security
date: 2018-09-02 21:58:09
---
### TL;DR

Don't trust your password on a system that has no multi-factor authentication mechanism, especially ones that allow the change of _the ID_ (email in this case) of your login without rigorous checks; *EVEN MORE SO* if you have your financial service access/information (such as credit card) associated to it.

### Update (Sep 07, 2018)

The account was recovered. That means I’m gladly proven wrong by DoorDash about not having sufficient logs to trace back. Thanks DoorDash.

<!--more-->

### How it started

On August 19th, 2018, at 1:28PM EDT, I received a disturbing email notification from DoorDash, indicating that I have changed my email address.

![Email alert](/images/screenshots/2018-08-19-email_change_alert.jpg)

As it says:

> If you initiated this change, you can safely disregard this email. If you didn't do this, please contact us by submitting a request here as soon as possible, since your account might be at risk.

That wasn't me, so I immediately took action to submit a support case as instructed by the email alert.

At this point I only have myself to blame, since I used my "throw-away" password for this account, which I'm 99% certain that it has already been "pawned".

Meanwhile I opened my banking app to check my credit card transactions since it was associated to the compromised DoorDash account, and of course:

![CC transactions](/images/screenshots/2018-08-19_cc_transactions.png)

The transaction was immediately reversed/refunded, thanks to DoorDash, and/or the underlying payment service that they use to process credit transactions; or even the person that compromised my account themselves -- it's possible that a hacker would want to prepare for "sleeper" compromised accounts by making the event as silent as possible to have a better chance of being unnoticed.

Whatever it was, I contacted my bank to get my credit card number re-issued.

### First response from DoorDash

I received the very first response from DoorDash support after about 5 hours. Probably due to my poor phrasing that mentioned about the above transactions (I don't have a record of the original message sent to them), and their support training of catching certain keywords to match cases to be handled, the emphasis understood by the support turned out to be ~~fradulent~~ fraudulent charges:

![First contact](/images/screenshots/2018-08-19_first_contact.png)

Then I responded by reiterating the actual issue, which was about my compromised account email address:

![First contact response](/images/screenshots/2018-08-19_first_contact_response.png)

And of course, the still misunderstanding response was that there were no records of transactions for both e-mail addresses:

![First contact 2](/images/screenshots/2018-08-19_first_contact2.png)

While their misunderstanding of the issue was frustrating to deal with, the information was somewhat meaningful, that the person who compromised my account may have already done a few more email changes to be virtually invisible without access to audit trails.

### Lessons learned

I suspect that either such audit trail does not exist for email changes (hopefully not the case, DoorDash), or that the consumer support team at DoorDash doesn't have access to such information.

<blockquote class="twitter-tweet" data-lang="zh-cn"><p lang="en" dir="ltr"><a href="https://twitter.com/DoorDash?ref_src=twsrc%5Etfw">@DoorDash</a> account got compromised, its system alerted me of the unauthorized email change, immediately followed up with a support ticket per instruction. ~29 hours later, still locked out of my account, and a compromised cc in it. Help!</p>&mdash; woozyking (@woozyking) <a href="https://twitter.com/woozyking/status/1031669919337742336?ref_src=twsrc%5Etfw">2018年8月20日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

This exchange of information, and my attempt of mild harassment on Twitter (no response to my public tweet though), or honestly just me reiterating repeatedly on the issue, and proofs such as my very first (and only) legitimate order through DoorDash, finally got a different kind of response from the support team -- they have elevated the issue to the appropriate team for review and follow up, and I can anticipate a response soon...

Or not soon enough, since it's been two weeks at the time of writing. Maybe that's their way of saying fuck off.

Because what does one frustrating consumer matter to a company that is valuated at [4-billion](https://techcrunch.com/2018/08/16/doordash-4-billion/), right?

But this event has taught me a few lessons, and I think some of them are especially valuable to me as a web developer, to be able to design and implement better systems that actually give a fuck about user accounts.

#### Leverage SSO (single sign-on) when applicable

I was afraid of sharing too much personal information, and being "vendor-locked" to the SSO provider -- but since I used my Gmail to sign up DoorDash originally, using its supported Google SSO would be a better choice. And who am I kidding about sharing personal information? I've already sold my soul to these internet giants `¯\_(ツ)_/¯`

#### Apply 2FA/MFA (multi-factor authentication) wherever possible

If it's a must to use a direct email login, make sure to also have 2FA/MFA in place to have an added layer of security.

I would even abandon the use of a traditional password login system, as the service provider is always at risk of getting hacked and have users' passwords being cracked. A one-time passcode based authentication system is less prone to such issue, since the generated passcode typically has a very short expiration time (usually within a few minutes), and a strong enough hash algorithm renders any cracking effort within such short window not worthwhile.

In fact, that's what I have done since 2014 for all projects that I've designed and implemented, to use email/SMS-bound "passwordless" authentication, with the assumption of:
- Any user can forget their password at some point
- Any password can be cracked given enough time

This way, there's no need to implement a "forget my password" mechanism -- it's already baked in. The mild inconvenience of having to receive a new passcode every time users attempt to login can be cleverly resolved through the techniques of what Slack calls "Magic Link".

But I would also offer an optional MFA on top of the "passwordless" login mechanism, implementing ["TOTP" (Time-based One-time Password Algorithm)](https://en.wikipedia.org/wiki/Time-based_One-time_Password_algorithm) through apps such as Google Authenticator doesn't add too much complexity to the system, but in exchange you get an even better layer of security for the users.

#### Separate emails from actual account IDs, and log EVERYTHING

Even I have fallen for being lazy on this. But my excuse is that I have never had a use case for users to change email addresses. But if your system does allow it, _please please please_ separate them from actual account IDs, and log EVERYTHING that can be offered as a trail when a compromising event like mine on DoorDash takes place.

Otherwise, kiss bye-bye to that account (and probably that service) like I did 👋