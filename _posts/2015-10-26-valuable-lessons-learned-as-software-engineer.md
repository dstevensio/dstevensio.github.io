---
layout: post-advicetweet
title: Valuable Lessons I've Learned as a Software Engineer
category: Advice / Retrospective
question_asker: Adri Van Houdt
question: the most valuable lessons you have learned and wish you knew from the beginning?
question_link: https://twitter.com/AdriVanHoudt_/status/658723715425914884
medium_url: https://medium.com/@dstevensio/valuable-lessons-i-ve-learned-as-a-software-engineer-63344931ff9d
---

## Share your code and invite review

At various early points in my career, I had the good fortune to join teams with exceptional engineers around me. (This continues to happen to me and is very true today, I only started with "various early points" as it's relevant to this post). As such, I often found it daunting to share my code with them and had to force myself regularly to invite their scrutiny.

As soon as I did, however, the quite unsurprising (in retrospect) result was that they gave me constructive criticism, the occasional jab (all in good humor) and increasingly with time, praise. Each time I opened my work up for feedback, I learned something. The next piece of code I wrote was better for it. This gradual, incremental improvement made me a better engineer.

## Review others' code, from the start

A companion point to the last: even when you're new (to the industry; to the company; to the discussion) you should participate in code reviews in the role of a reviewer.

For a long time, I stayed away from commenting on or asking questions of my colleagues' code because of that same feeling of inadequacy in comparison. But that ignored two critical benefits of the Code Review process:

1) The chance for everyone to learn and improve — if I ask a question about how they did something, or suggest an alternative, I can either be correct, or have spotted something awry; or, I could be wrong, perhaps mistaken. In both cases, someone learns and someone improves. If the code review is taking place on a github issue or somewhere else that's archived and accessible, others not participating can also learn from it.

2) The chance to offer new perspective — no matter what you've done in your career or your life prior to the moment you enter your thoughts on that Code Review, you have had different experiences and have been exposed to different things than anyone else participating. No two people are ever exactly the same. Maybe you spot something no-one else would, or identify the danger of an issue that you've faced before that due to that experience, only you can spot the conditions under which it would occur.

Speak, and speak early. No one is going to attack you for giving honest feedback, asking questions and helping other engineers learn and grow (even if they're learning to defend their approach — that's a great skill to have also). And if you do get attacked or belittled, well — at least you know you're working with a bunch of a-holes and it's time to move on.

## Production Errors aren't the end of the world

You're going to contribute a lot of code in your life. On multiple occasions, that code will go in to production and break something. Sometimes small things, sometimes super-critical things. There is no engineer in history that has not experienced exactly that.

You should work diligently and intentionally to write the best code, supplement that code with accurate, complete and relevant tests, and embrace a robust QA process in a staging environment to minimize the risks of having this sort of thing befall you.

But it will still happen from time to time. Don't let the fear of that stop you from trying things, from making bold moves, or taking on big projects. I have a little secret for you:

<strong>If you push something breaking to Production, you make sure you know about it ASAP, then you make sure you fix it in the shortest time possible. Most importantly, you store the memory of what you did and why it broke, and you let that inform your future efforts.</strong>

I've worked with colleagues who've accidentally dropped the entire customer database table from the production MySQL instance (in a company that really, really sucked at backing data up); one who deployed a redirect loop to a high-traffic site; and one who took down an App-Store-Top-10 iPhone app for 2 hours due to a typo. None of them got fired. And nor should they. Be bold. Just make sure you learn from each chest-clenching moment and let it improve you for next time.

## You'll never predict the language or framework that will "win"

You can't pick the "right" language to learn or the "correct" framework to use. That's because neither exist. Learn the fundamentals of programming, get really good at solving problems with the right tool, and then apply those skills to whichever language, framework or other that is called for by the set of circumstances you find yourself in.

Should you learn Rails? I don't know, are you working somewhere or for someone who uses Rails for their application? How about node.js — is that the one to learn? Again, will you need to know it? Are you learning it because it looks fun, or cool, or interesting, or for the hell of it? Those are all good reasons too.

Good Engineering is just problem solving. How you solve it will depend on the time, the place, the preference of your boss, or your team, or some faceless head-office directive, or the toss of a coin. It could be anything. Don't tie yourself down to one specific thing unless you want your entire career to be guided by where you can do that one specific thing.

## Resist the urge to use a GUI

When you are exposed to a new tool or piece of software and you don't quite get it yet, it can be tempting to use any available GUI. Think Github's Desktop Client, or any of the multitude of MySQL GUIs. Pretty soon, you'll be accustomed to using them and will probably even associate them with the underlying task you are using them for.

However, there are three major drawbacks to using a GUI rather than the command line:

### 1. Depth of understanding
Pointing and clicking tends to become muscle memory pretty fast, and GUIs often use "more human" terminology for commands. Combining these two means that often, you end up without a deep understanding of what you're actually doing and what's really going on under the hood.

### 2. Availability of that GUI
If a server or a computer you use has something you need to interact with (git, MySQL, etc, etc) then it will have a command line tool to interact with it, as standard. The same cannot be said of the GUI. Don't introduce a blocker for yourself — we're all in this game to get work done, set yourself up for success!

### 3. Restricted feature set
I will gladly be corrected with any examples you can think of, but in my experience I have yet to find a GUI that exposes exactly the same number of features, options, flags, etc as the equivalent command line tool. There's often so much more configuration that can be achieved via the command line — don't sell yourself short or limit your options.

Oh and on top of that, the command line tools are always considerably faster — and can be scripted. Save yourself valuable time!

<strong>Bonus tip:</strong> learn Vim. It's available on just about every server you'll ever end up SSH-ing in to, you can configure it on your machine to create workflows you positively salivate over, and if you take a few moments to learn the shortcuts, you will shave minutes off of navigating around your file as you edit. Seriously, learn Vim and use it as your primary text editor / coding tool / IDE. Thank me in a year.

## Get comfortable with BASH and BASH Scripting

You're going to spend a lot of time in the terminal. Learn the shortcuts that will save you hours (<em>over the course of your career</em>) [example: Ctrl + A to jump to the start of the line; Ctrl + E to jump to the end of the line].

Then learn some simple scripting (or some advanced scripting if you like it, don't stop because I said so...) so you can make actions you need to take easily repeatable (by you, and by others). This also helps you reduce the number of times you accidentally forget to take an important step in a process.

## Whatever you think is important, what's actually important is getting things live and making them work really, really well.

No one will care in 6 months that you built this with technology X, framework Y or with workflow method Z. Pick something by making an informed decision, then work to make your end product really, really resilient, performant, and for the love of all that is holy, make it solve the problem you set out to address.