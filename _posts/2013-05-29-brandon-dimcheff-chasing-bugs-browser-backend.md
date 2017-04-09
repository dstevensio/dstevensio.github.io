---
layout: post-talk-jsconf13
title: Brandon Dimcheff - Down The Rabbit Hole - Chasing bugs from the browser to the backend
category: JSConf US 2013
youtubeID: RCgD4SLwC18
---

When a bug occurs/is reported, there's always these stages

Denial
---
- there's no way this is happening (PEBKAC)
- listen to users

Acceptance
---
- How often is this problem happening?
- logs
- Graph things to see scope of problem
- unix: jsontool (npm install jsontool)

go back to logs find info

(log everything! more data = more chance of tracking issues down)

Reproduce
---

- how can you reproduce scenario in which error occurred?
- do the stupidest, easiest thing that works (no rabbit holes)

Diagnosis
---

- chrome dev tools
- wireshark

"shotgun debugging" - breakpoints EVERYWHERE

Fix
---

Lessons
---
- Graph everything
- log everything
- do stupidest, easiest thing that works rather than fancy solutions
- learn your tools
- listen to your users (no more denial!)

Twitter: [@bdimcheff](http://twitter.com/bdimcheff)

Brandon is from Olark - [they're hiring](olark.com/jobs)

