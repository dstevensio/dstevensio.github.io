---
layout: post-sss
title: Denial of Service (DDoS)
category: Bare Minimum Application Security
tags: postlist
medium_url: https://medium.com/@dstevensio/barebones-application-security-denial-of-service-805a73b5bc70
---

In this final look at real world threats, it's time to look
at an operational issue, Denial of Service. Previously after
[introducing the series](/startup-or-bare-minimum-security),
we looked at [XSS](/startup-or-bare-minimum-security-part-2-xss),
[CSRF](/startup-or-bare-minimum-security-part-3-csrf),
[SQLi](/startup-or-bare-minimum-security-part-4-sqli) and most
recently, [Spammers](/startup-or-bare-minimum-security-part-5-spammers).

(D)DoS - (Distributed) Denial of Service
----------------------------------------

### What?

An attempt to render your app/service inaccessible or unusable
by overwhelming it with bogus requests/connections such that it
is incapable of serving legitimate requests/connections.

The Distributed part means that instead of the attack coming
from a single source (easily blocked and mitigated against),
the attack comes from a huge number of sources (think: botnet)
to make blocking/tracing difficult.

### Real Example

Imagine a Naughty Hacker adding this to their machine's cron:

{% highlight bash %}
* * * * * curl http://insta-spam.com
{% endhighlight %}

Every minute of every day of every year, it'll request the
homepage of insta-SPAM. 1,440 requests per day. No big deal,
we can handle that. Now let's say NH adds this to the cron on
250,000 machines they control through a botnet. 360,000,000
requests per day. Little bit trickier to handle. Now let's
say they actually run a script that hits a URL on insta-SPAM
that causes the server to return a 500 error. That's more taxing
than just returning a known page. 360m requests every day using
resources due to an error condition. Where are we hosting
Insta-SPAM? AWS? Heroku? Hope I'm not paying that bill!

So, protecting against this risk. First, caching. Cache your
pages wherever possible. Cache them at a third party like Akamai
if you can. Monitor access and drop traffic from IPs that are
sending a high volume of requests, not in keeping with your
normal app usage. Avoid exposing any endpoint in your app that
takes a long time to respond (indicating that it is doing
something intensive).

### Why you should care:

Availability is key. If a user can't access your app, they
won't use it. If they don't use it, they won't realize it's
the greatest thing ever. Ergo, they won't come back. Want to
raise capital? Great, enjoy explaining why the investor who
was super excited about pouring in money to your startup can't
pull up your homepage, never mind the cool parts of your app
that would have sold him/her on its promise.

### How to detect if someone is trying to attack you

Massive spikes in traffic. Yes, you may have hit the homepage
of Hacker News. Yes, your viral marketing campaign may have
hit the spot. But be realistic. Is there any logical explanation
for a 1000% increase in traffic? Look at the types of requests
you're seeing. Look at where they're coming from.

### Non-security benefits of protecting yourself from this threat

(D)DoS mitigation is often considered an operational task versus
a security one, because essentially you're ensuring your app can
handle massive amounts of traffic. Get these steps right, and once
you've scaled to a billion users, you're going to be well equipped
to serve them.

End of Series
-------------

So that concludes our high level overview of the specific threats you'll face
from day one, potentially.

And, as a reminder - I will continue to repeat a disclaimer throughout: This is a barebones, do this
rather than do nothing set of suggested approaches. **THIS DOES NOT CONSTITUTE
ROBUST, COMPLETE AND FOOLPROOF SECURITY**. The goal of this effort is to provide
non-security aware founders/hackers/developers/etc with a modicum of protection
at a stage in the company's growth where there are no budgets, let alone one
for Information Security. The caveat is that as soon as the company experiences
growth, one of their top priorities should be to mature in to a properly
developed, professionally and thoroughly provisioned Information Security
program, specific to their application, industry and environment.

Just as you scaffold certain items while doing rapid coding development, this
is your scaffolded application security program. Think of it as the Twitter
Bootstrap of web application security.
