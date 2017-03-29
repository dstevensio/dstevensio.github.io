---
layout: post-sss
title: Startup or Bare Minimum Security Part 3 - CSRF
tags: postlist
---

In [Part 1 of this series](/startup-or-bare-minimum-security) I began by
explaining the background to the series, my aims and goals in producing
this, and kicked things off with some core concepts.

[Part 2 focused on Cross Site Scripting (XSS)](/startup-or-bare-minimum-security-part-2-xss)
and today we continue with an issue affecting sites that accept user data.

CSRF - Cross Site Request Forgery
---------------------------------

### What?

CSRF is the act of manipulating a form, or API endpoint, etc that stores
or acts upon the action of a user, to initiate that action such that it
appears a certain user initiated it, when they did not.

### Real Example

{% highlight html %}
<form action="/save/user_account_email" method="GET">
  <input type="text" name="email">
  <button type="submit">Save Email Address</button>
</form>
{% endhighlight %}

Seems legit - we let the user update their email address on file with this
handy dandy form. But what if we do this:

{% highlight bash %}
$ curl -G -d email=naughtyhacker@gmail.com https://insta-spam.com/save/user_account_email
{% endhighlight %}

No problem you say, as we grab the user ID from the active session to work
out which account we're updating. When we cURLed it, there was no valid
session associated with the request so the request was not acted upon. Victory!

Cool story bro.

Unfortunately for you, this naughty hacker knows a few email addresses of
folks who have accounts on Instaspam, due to public email addresses of
bloggers who have been giving this hot new startup some coverage. He/she
also knows about CSRF. NH sends an HTML email to those email addresses 
which includes the following img tag:

{% highlight html %}
<img src="https://insta-spam.com/save/user_account_email?email=naughtyhacker@gmail.com">
{% endhighlight %}

Any recipient that has an active session on insta-SPAM and opens the
email in their web browser has now handed over their account to the naughty hacker.

So what we **should** have done is a tokenization approach for form submissions
or API calls. When you generate your form, generate a unique token that can be
passed in a hidden form field, and store that token along with the authenticated
user for whom it was created, and a time of expiry (TTL) that makes sense - usually
15 mins to an hour, depending on what the form does and how long it takes to fill
it out. On your backend, when the controller that handles the form submission
receives a request, it checks that the token is present, and that the token is still
valid. If so, it processes the request. If not, it drops it.
For API calls, you should have a call that creates a request token. All future
requests send this token in addition to the request data, until such time that
the token expires and they request a new one. Same principle as before.

### Why you should care

A CSRF vulnerability gives an attacker the opportunity to impersonate your users.
On the low end of the scale, this could mean your user has a poor or confusing
experience on your app (information they entered deleted or altered, getting 
logged out mid-session, etc) or at the scarier end, allowing the loss of credit
card data, personally identifiable information, personal data/content.

### How to detect if someone is trying to attack you

CSRF attacks often stand out because your application flow isn't followed.
Consider the example we just had of changing the user email address. If you
receive a request to that controller without having seen a request to the view
that contained the form, that's an indication that something might be wrong.

**DON'T** rely on checking the referrer to see if the form was submitted from
your app though - remember that the referrer, as an HTTP header, can be
manipulated by the attacker. Log in the controller that served the view with the
form in it that the user requested that view, with a timestamp, and look for one
of those log entries for every log entry of a request to the form submission controller.

Once you have tokenization in place, check your logs for requests without a token.
Then also check for requests with expired tokens - many will be legit (page
refreshes, etc) but some will indicate an attempted attack.

### Non-security benefits of protecting yourself from this threat

You may see some reduced erroneous form submissions - such as someone's session
expiring and then hitting a form that was still open in their browser.

End of Part 3
-------------

Hopefully you now have a basic understanding of the threat of CSRF and
ways to protect yourself. [In Part 4, we'll look at
SQL Injection (SQLi)](/startup-or-bare-minimum-security-part-4-sqli).

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
