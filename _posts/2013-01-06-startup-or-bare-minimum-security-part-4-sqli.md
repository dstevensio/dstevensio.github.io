---
layout: post-sss
title: SQL Injection (SQLi)
category: Bare Minimum Application Security
tags: postlist
medium_url: https://medium.com/@dstevensio/barebones-application-security-sql-injection-sqli-4becb8baf604
---

We're roughly halfway through this series on basic security
steps for Startups to take. After [introducing the series](/startup-or-bare-minimum-security),
we then covered [Cross Site Scripting (XSS)](/startup-or-bare-minimum-security-part-2-xss) and
then [Cross Site Request Forgery (CSRF)](/startup-or-bare-minimum-security-part-3-csrf).

SQLi - SQL Injection
--------------------

### What?

SQL injection vulnerabilities allow an attacker to modify a SQL
query in your app to perform an unintended and undesired action.

### Real Example

Imagine we have a search page in our app. Here, we're allowing
people to search for types of food replaced by SPAM in Insta-SPAM.

{% highlight html %}
<form action="/search" method="POST">
  <input type="text" name="term">
  <button type="submit">Search</button>
</form>
{% endhighlight %}

And in our form submission controller, we have this:

{% highlight php %}
$term = $_POST["term"];
$sql = "SELECT * FROM photos WHERE description LIKE '%" . $term . "%'";
$res = mysql_query($sql);
{% endhighlight %}

When our nice, kind users are enhancing their mood with Insta-SPAM,
they might search for instances of Burgers being replaced by tins
of Spam, by entering "Burgers" in the search box. Here's what the
SQL query looks like when we execute it:

{% highlight sql %}
SELECT * FROM photos WHERE description LIKE '%Burgers%';
{% endhighlight %}

Now consider our Naughty Hacker visiting Insta-SPAM and
entering this in the search form:

{% highlight sql %}
cheese%'; DROP TABLE photos; DROP TABLE customers; DROP TABLE users; --
{% endhighlight %}

I'm sure you know where we're going with this but this
is the SQL created that we then run:

{% highlight sql %}
SELECT * FROM photos WHERE description LIKE '%cheese%'; DROP TABLE photos; DROP TABLE customers; DROP TABLE users; --%';
{% endhighlight %}

NH has dropped our photos table! All that day-making content, GONE!
He/she has had a stab at dropping our customers and users tables - we've
probably stored our user base in one of those right?!

So, to prevent this catastrophic series of events, use parameterized
queries where supported, make sure you validate, sanitize and escape
where necessary, and never, ever, use string concatenation to build
SQL queries!

### Why you should care

Data loss is catastrophic. Loss of data integrity is dangerous. And
if you let an attacker run whatever SQL they like on your database,
you give them a free pass to do whatever they want with one of your
most critical assets.

Also remember to be smart about the permissions you grant to the
database user that the app uses. Does your user that runs select
queries to power your search need to have privileges to drop tables? NO!

And... no, you don't need reminding about this one. Right? You're not
running your production database access from your publicly available
app as... root? Right? Of course you're not. DON'T RUN AS ROOT.

Purely using a NoSQL solution for data storage? Awesome! I love
redis, I like couchDB and I've heard Mongo's a hoot. No need
to worry about SQL injection then eh! Well, yes, if we're being
pedantic you will not be at risk to SQL injection. But these
hackers are industrious and love to learn. They are well aware
of what can be done with NoSQL too. VALIDATE. SANITIZE. ESCAPE.

### How to detect if someone is trying to attack you

Log the input to form controllers and anywhere that accepts
user input, then look for patterns that match SQL statements.
It's not a difficult task to write a regex that looks for the
types of queries that will cause you problems.

### Non-security benefits of protecting yourself from this threat

Ensuring you are constructing queries correctly is good programming
practice, and maintaining the correct level of permissions/privileges
for database users is just good sense.

End of Part 4
-------------

Your data is critically important to you and to those whom it pertains to -
so taking steps to ensure the easiest way an attacker can steal that data
is mitigated is an important part of this effort. [In part 5, we'll look at
the threat of Spammers](/startup-or-bare-minimum-security-part-5-spammers) -
and how to stop yourself from becoming a weapon in their arsenal.

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
