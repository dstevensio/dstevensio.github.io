---
layout: post-sss
title: Spammers
category: Bare Minimum Application Security
tags: postlist
medium_url: https://medium.com/@dstevensio/barebones-application-security-spammers-397af2b67ea4
---

So far in [this series](/startup-or-bare-minimum-security) we've
tackled [XSS](/startup-or-bare-minimum-security-part-2-xss),
[CSRF](/startup-or-bare-minimum-security-part-3-csrf) and
[SQLi](/startup-or-bare-minimum-security-part-4-sqli). Next, we'll
look at the threat of unsolicited mail.

Spammers
--------

### What?

Those nasty people that send you emails suggesting you need
online dating sites, prescription meds without the
prescription, or offering you the once in a lifetime
chance to become a millionaire just by allowing the use of
your bank account to harbor vast fortunes of Nigerian princes
who seem to be fleeing their country at an astonishing rate.

As a sidenote, Nigeria is a federal republic modeled after
the US (once it gained independence from us Brits of course)
and doesn't have a royal family. Nor did it within the
reasonable lifespan of anyone who could have been a prince.
Anyway...

### Real Example

On Insta-SPAM, we have a tell a friend feature, so that our
users can share the love with others they know, driving
traffic to our app.

It's a simple form that lets you enter an email address and
a personal message. We then send an email to the entered
address with the message and a link back to Insta-SPAM.
Marketing!

Spammers find this form, thank us kindly while high fiving
each other, then write a script to post to this controller
with thousands of email addresses and their spam payload.

### Prevent this threat by:
* Consider if you need to allow customizing of the message.
Is it really necessary? Instead of letting them enter a
personal message, why not craft a simple, unobtrusive but
catchy message for your users, thereby removing the input
that could be abused.
* If you do have a message, limit the length of what can be
entered, and don't allow HTML or links.
* You can use a CAPTCHA if you want, but they are the worst
UX of all time. Instead, rate limit the number of emails that
can be sent in a given time period, limit the number of email
addresses that can be put in the email address field of your
form, use tokenization as in the CSRF step to prevent scripting.
* Have your mail sending controller check for patterns that
suggest spam and flag for review before sending. And always
use a message queue with a way to halt sending, so if you
get hit by a spam attack, you can turn off processing of
the message queue, clean it up, then turn processing back on.

### Why you should care

Spam is the bane of the internet. Don't help it propagate.
You'll also open yourself up to Phishing attacks either
with your users as a target, or as a method of delivery
to other victims. Don't introduce that liability.

### How to detect if someone is trying to attack you

Monitor your message queues. Multiple messages with similar
or identical content (that isn't the content you insert) are
likely spam. A huge spike in the number of messages in the
queue might indicate spam. And it's a great idea to have
your mailer send a copy of every sent message to an account
you can look at. That way, you get the same experience as
the recipient, and can quickly spot spammy emails.

### Non-security benefits of protecting yourself from this threat

It'll force you to really think about your tell a friend
feature. Then you can make it not suck. (Seriously, most
of them suck.)

End of Part 5
-------------

Email, an extremely old technology, is still super useful today - so anything
we can do to stop giving those that seek to ruin it an easy method to do so
is well worth it. Not to mention your savings in wasted sent messages! In
[part 6](/startup-or-bare-minimum-security-part-6-ddos), we'll round out
our drill down in to specific threats by looking at (Distributed) Denial of
Service ((D)DoS) attacks.

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
