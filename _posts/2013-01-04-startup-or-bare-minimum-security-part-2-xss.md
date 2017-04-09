---
layout: post-sss
title: Cross-Site Scripting (XSS)
category: Bare Minimum Application Security
tags: postlist
medium_url: https://medium.com/@dstevensio/barebones-security-cross-site-scripting-xss-921f9187814c
---

In [Part 1 of this series](/startup-or-bare-minimum-security) I began by
explaining the background to the series, my aims and goals in producing
this, and kicked things off with some core concepts. Here on out, we'll
focus on specific threats for the next few posts.

XSS - Cross-Site Scripting
--------------------------

### What?

An XSS vulnerability allows us to run arbitrary javascript on the vulnerable app.

### Real Example

Consider this superbly designed, aesthetically pleasing page on your site:

{% highlight html %}
<html>
<head>
  <title>Insta-SPAM</title>
  <script>
    window.isTest=1;
    var _gaq = [];
  </script>
</head>
<body>
  <script>
    if (window.isTest) {
      document.write("<a href=\"#\" onClick=\"_gaq.push(['_trackEvent', 'AttractiveLink', 'Clicked', '" + window.location.href + "']);\">Click My Attractive Link</a>");
    }
  </script>
</body>
</html>
{% endhighlight %}

Being all lean and data-driven, we're doing some A|B testing. Imagine, for the
sake of simplicity, that `window.isTest` is being set by whatever A|B testing
solution we are using, and that the `_gaq` array is from the Google Analytics
tracking code. We've got a feeling that attractive links are the way forward
for driving user engagement, so we're testing that by writing an attractive link
to the page for users in our test group.

We want to track how many people click on this attractive link we’ve set up. We
are going to put it on a few pages to determine where it performs best, so our
final value sent to the Google Analytics event tracking array here is the page URL.

We like using templates to avoid shared code, so we use window.location.href to
populate the page URL easily. Great! Check out our efficiency.

When a user visits our homepage, on which we have placed this code:

    http://insta-spam.com/

We see this behavior when the user clicks the attractive link:

{% highlight javascript %}
_gaq.push(['_trackEvent', 'AttractiveLink', 'Clicked', 'http://insta-spam.com/']);
{% endhighlight %}

However, we have a problem. window.location.href contains whatever is in the
browser's address bar - which is editable by the user directly and via a link in
an email, Facebook message, etc. Uh oh!

Imagine a l33t hacker targets your site (for this, and future examples, we're
imagining our new start up Insta-SPAM, which pulls photos of food from Instagram's
API and overlays a picture of a can of SPAM on the food item - it's going to be
massive) and sends an email to a bunch of people linking to:

    http://insta-spam.com/?']);alert('Insta-SCAM!');//#

This causes the `gaq.push()` argument to be:

{% highlight javascript %}
_gaq.push(['_trackEvent', 'AttractiveLink', 'Clicked', 'http://insta-spam.com/?']);alert('Insta-SCAM!');//#']);
{% endhighlight %}

We failed to validate or escape the location.href value and now, the l33t hacker is
slandering us! And of course that's just one example. If the bad guy/gal can run any
JavaScript they choose, the list of things they can do is extremely long - write
script tags to the page that pull in external scripts that gives control of the
browser to a remote attacker (BeEF), steal cookies, the list goes on.

### Why you should care

The power of JavaScript and the range of things you could see happen to your users and
to other visitors to your app should be enough cause for concern, but also bear in
mind that even if you have nothing worth stealing in your opinion, an XSS vulnerability
allows people to use your site as part of an attack on others (want to explain to the
DoD why attack traffic is coming from your server?) or explain to an investor or customer
why your homepage is advocating anti-semitism or promoting racial hatred?

### How to detect if someone is trying to attack you

It's important to understand that there are two distinct types of XSS - reflected and stored.

A reflected XSS vulnerability is one that can be triggered with access to the victim - a
vulnerability that exploits unescaped use of location.href, or of a URL parameter, for
example - that the attacker must get in front of the victim.

A stored XSS vulnerability is even better for the attacker and stems from a vulnerability
in content retrieved from a data store, so imagine a web forum that doesn't validate and
sanitize the input of messages, nor escape the output. The attacker just posts the
JavaScript he/she wants to execute in a popular thread, and hey presto - every time
another user views that thread, the script runs and the attacker gets another victim.

So, detecting these attacks is done in one of two ways.

To detect reflected XSS attacks/attempts, check your access logs often. Automate this
looking for strange patterns (you know your app inside out - alert on URL patterns that
don't fit your design). A quick and dirty solution that gets the job done is achievable
in an hour or two. Contact me if you want my help in creating one.

Before long, you'll likely be monitoring failing URLs for operational reasons anyway -
oftentimes XSS attempts will crop up in the list of URLs that tripped up.

For stored XSS attacks/attempts, you can detect at multiple layers of your stack:
periodically querying your data store for fields that accept user modifiable input,
logging any matched patterns on the way out in your view logic, or on the controller
that processes incoming form data.

Why bother detecting attacks if we're validating, sanitizing and escaping? Good question,
I'm glad you asked. Taking measures to detect attacks puts you ahead of the game:

* You may detect something that your validation/sanitization/escaping efforts will not catch, allowing you to improve them proactively.
* Being aware of failed attack attempts allows you to be braced for further attacks, determine attack sources and enlist further help when needed to ensure any follow up attack(s) are not successful.

### Non-security benefits of protecting yourself from this threat

I like to tell developers that taking the time to stay abreast of security best practices
benefits them beyond the scope of being secure. I figured I should back up that bold
statement with examples.

By validating all data input in your app, you become acutely aware of the data types you're
using. That will at the very least give you a better understanding of your app and the
ability to better design data structures, and may even extend you opportunities to identify
inefficiencies in your tech stack, which you can then address.

Also, if you're not a JavaScript person, detecting the attempts at XSS and seeing what
they can do with JS will likely give you a new appreciation for the language, or if
nothing else, teach you more about the way it works.

End of Part 2
-------------

So, that's XSS in a nutshell and a few ways to protect yourself. In [Part 3, we'll look at
Cross-Site Request Forgery (CSRF)](/startup-or-bare-minimum-security-part-3-csrf).

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
