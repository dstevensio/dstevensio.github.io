---
layout: post
title: Which Auth?
category: Application Security
---
We like to shorten long words. It saves us a little time talking, or a little time typing, and in meetings we can sound like we're a real big shot because we use the shortened form.

However, there are times when this predilection for brevity can harm us and the understanding we have, or the understanding we convey.

Let's take a look at two concepts from the field of Application Security to demonstrate the danger:

<em>Auth</em>entication and <em>Auth</em>orization.

Think back to the last time someone in your organization was speaking on the issue of handling users and their access to a system. Am I right in thinking that they may have said "<em>we auth the user</em>"?

A problem with the understanding that results is that many of us who grasp both concepts can flip to the correct meaning upon hearing that sentence, but for newer folks or those who haven’t been exposed to security considerations, the two are often conflated together.

Why is that a bad thing? First, we should define what we mean by each term:

<strong>Authentication</strong> is the act of verifying whether a user has access to a system, and concerns their identity. <em>Who is this, and are they allowed in?</em>

<strong>Authorization</strong> is the act of verifying whether a user is allowed to perform an action, and concerns their privileges. <em>Is this user allowed to do that?</em>

Let's consider a hypothetical example where conflating these two to be the same concern is dangerous.

We have a messaging application that performs three tasks. Messages can be created, Messages can be deleted, and Messages can be viewed.

These messages concern internal business at an organization. The information contained within the messages must remain private to the organization, and within the organization, and is for use by full-time employees only.

Assume we have four users:

Lauren, who is an employee sending and receiving messages as part of her day to day work.

Anil, who is also an employee sending and receiving messages as part of his day to day work.

Jaime, who is an employee responsible for the messaging application itself.

Aaron, who is a contractor working with the organization but does not send or receive messages as part of his role.

First, the Authentication scenarios:

Lauren, Anil and Jaime, when attempting to log in to the messaging application, should be successfully authenticated with their credentials and provided access to the application, as long as their credentials (username and password; 2-factor authentication solutions like tokens or codes emailed/SMS'd/etc) are correctly provided.

Aaron, when attempting to log in to the messaging application, should be rejected and will not be authenticated, even if his credentials are correctly provided (as he is not a full-time employee).

Next, the Authorization scenarios:

Lauren and Anil should be permitted to:

* Create a new message
* Delete their own messages
* See messages addressed to them

To be overly simple, Lauren and Anil are regular users.

Jaime should be permitted to:

* Do everything listed for Lauren and Anil above, plus
* See all messages in the application
* Delete any message in the application

Again being simple in terminology, Jaime is an Administrator or Super User.

If Lauren or Anil try to view messages that are not addressed to them, or delete messages other than their own, they should be prevented from doing so because they are not <em>Authorized</em> to perform those actions.

If the implementation of the messaging application conflates Authentication with Authorization, Lauren and Anil could log in and then view messages not addressed to them, and delete messages other than their own.

So, next time you’re planning an application or system and you hear someone say they've handled auth, be sure to verify their understanding of the two concepts we shorten to auth and the importance of both working together to secure your application.
