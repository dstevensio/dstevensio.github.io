---
layout: post-sss
title: Startup or Bare Minimum Security
tags: postlist
---
I gave a presentation for some folks who were setting up side projects
outside of work hours, some of them fully fledged startups, some even at the
point where they'd left to go full time to pursue their ideas.

The topic of the presentation was Information Security, or more specifically,
(Web) Application Security. I worked as an Application Security
Engineer for several years and having been exposed to the many different facets
of this industry, the overwhelming trend I've noticed for the field is that:

a) Advice for the individual developer seems almost always written by a
Security Professional who writes in a manner best understood by other Security
Professionals; and

b) Everything seems to be written around the premise that you do ALL the best
practices without fail, immediately, with a heavy lean toward Enterprise scale
audiences.

I'm generalizing, of course. There are some fantastic people doing their bit to
make sure sound advice is available to those who need it. However, publicizing
where to look and finding a single, concise place to read up on all the
relevant topics is difficult and oftentimes daunting for developers with little
to no security exposure.

So, I threw together some thoughts on the immediate issues a budding new
company will face in the sphere of Web Application Security and some practical
*Bare Minimum* steps a small, new company/project can follow to protect
themselves against some common threats.

I will continue to repeat a disclaimer throughout: This is a barebones, do this
rather than do nothing set of suggested approaches. THIS DOES NOT CONSTITUTE
ROBUST, COMPLETE AND FOOLPROOF SECURITY. The goal of this effort is to provide
non-security aware founders/hackers/developers/etc with a modicum of protection
at a stage in the company's growth where there are no budgets, let alone one
for Information Security. The caveat is that as soon as the company experiences
growth, one of their top priorities should be to mature in to a properly
developed, professionally and thoroughly provisioned Information Security
program, specific to their application, industry and environment.

Just as you scaffold certain items while doing rapid coding development, this
is your scaffolded application security program. Think of it as the Twitter
Bootstrap of web application security.

What I aim to provide
---------------------
* Simple, Actionable DOs
* Easy, extensible DON'Ts (you'll thank me in a year)
* Real examples
* Links to Free Tools and Services
* An outlet for any questions/concerns you may have (Twitter me:
  [@dstevensio](http://twitter.com/dstevensio))

What I intend to avoid
----------------------
* Preaching
* Getting bogged down in details

This should be
--------------
* Brief & Concise (within reason)
* Useful
* Empowering

Threats you should actually care about
--------------------------------------
### (because you will probably face them soon)

These are the threats I'll be covering. As we go over each, I'll explain what
they are and give you some ways to protect your application from them.

* XSS
* CSRF
* SQLi
* Spammers
* (D)DoS

But first... general concepts
-----------------------------

I'm avoiding going in to gut-wrenching, eyeball-straining, sleep-inducing detail,
but let's first cover some general concepts to bear in mind while launching
something that is going to make you fabulously rich, or humanitarian of the
century, or both.

In other words - if you forget the entire content of this except for these core 
concepts, at least you'll be better off than the majority of hot new startups
(and a huge number of established, sprawling corporations that really should know better)

### TRUST

Because you're a cool startup, you're going to trust your users (customers,
[sorry Jack Dorsey](http://jacks.tumblr.com/post/33785796042/lets-reconsider-our-users)).
This trust will be bred through the open communication you'll have with them.
They will feel like they know you and you will feel connected to them.

However, there are bad people in the world. So trust your users/customers but 
don't trust the input they are providing. **ANYTHING NOT HARD-CODED IN YOUR
APP CANNOT BE TRUSTED AS SAFE**. This extends to values pulled from your 
database, your NoSQL store, whatever datastore you use.

**DO**: Validate & Sanitize on the way in, Escape on the way out

**DON'T**: Trust anything that isn't hardcoded

### HTTPS / SSL

If you are handling your own user authentication, the submission of user
credentials must always take place over SSL. There are freely available
browser add-ons to sniff traffic on a network, so even rank amateurs can
do man in the middle attacks to grab log in credentials. HTTPS protects 
you, and it's cheap and achievable.

If you're submitting sensitive information (anything personally
identifiable, anything payments-related, etc) it should be submitted 
over SSL without a doubt.

**DO**: Send all sensitive/personally identifiable information over HTTPS not HTTP

**DON'T**: Use a self-signed cert (you don't want to train your users to get used
to accepting security exceptions as you're making them less protected against future issues)

### IF YOU DON'T NEED IT DON'T STORE IT

Every piece of information you collect about a user/customer adds to
the value of collective information held in your data store. Collect
whatever information you feel you need to provide a great experience
to the user and to show value to your investors, protect that information,
but if you have no current or (predicted) future need for a piece of
information, don't collect & store it. That introduces unnecessary risk.

**DO**: Collect necessary data and protect that data

**DON'T**: Store information for the sake of it

### IF SOMEONE DOES IT BETTER THAN YOU LET THEM DO IT

Relying on a third party is a dangerous proposition. If something as
critical as logging in to your site is handled by servers, code and
humans outside of your control, you may be in for heartache if one of
those is unavailable or broken when a user tries to log in to your site.

However, if you feel a Facebook, a Twitter or a Google, for instance,
has a greater availability record than you expect to maintain and you
don't consider having local user accounts essential, letting them take 
care of authentication and user credentials removes an area of risk for you.

If you are accepting credit cards on your app, do you need the granular
control that having your own payment processing solution provides? Or
could you let the likes of [Stripe](https://stripe.com) take care of
that for you, negating the need for you to take on the responsibility
of storing CC info?

You'll make decisions based on your business need and what makes sense
for you, your users/customers and your investors, but make sure you
consider the benefits of letting truly sensitive data be handled by
someone with the resources and investment to adequately protect that
data with high level of confidence.

**DO**: Create your own software and services if required to meet your
business needs, taking adequate steps to protect those systems

**DON'T**: Reinvent the wheel if your expertise pales in comparison
to a provider of a service.

### BUT... DON'T ASSUME THEY'RE PERFECT

Hurriedly backtracking, I will attach one caveat to this though.
This does not absolve you of the need to abide by the other
concepts previously mentioned. **DO NOT TRUST** data from a third
party as safe. They will likely (if they're a reputable service)
take care of sanitizing, escaping, validating, etc. But don't 
leave it up to them. Validate the data is correct before displaying
it. Escape it where necessary. Don't submit data to a third party
over HTTP if the data is remotely sensitive. Don't let a Facebook
or a Twitter store data on your behalf that you don't need (think:
permissions models, what data you’ll ask the user to grant access to, etc).

**DO**: Validate, sanitize and escape data from third parties.

**DON'T**: Snuggle up in the warm fuzzy blanket of Security by
saying "oh, service *X* handles that for me so I'm not vulnerable..."

End of Part 1
-------------

That's a lot of information in one fell swoop, and I did promise not to go
in to information overload mode, so that concludes the first part of this
series of posts on Startup Security.

In [Part 2](/startup-or-bare-minimum-security-part-2-xss), we'll get stuck in to the threats themselves - starting with Cross Site Scripting (XSS).

