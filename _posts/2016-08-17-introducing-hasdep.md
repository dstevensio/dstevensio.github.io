---
layout: post
title: Introducing hasdep
category: Projects
---

Dependency management is a thankless task, and one that can easily get out of control in a modern JavaScript project. Consider the need and desire to standardize dependency versions across a large number of modules in a large organization and it’s usually not long before you’re banging your head against the wall.

Progress is awesome. It’s especially awesome when it comes to build tools, transpilers, test runners and the like — we are advancing at a great rate. But what happens when you have utilities (like lodash), server application frameworks (like hapi) or core libraries (like react) that are frequently improved upon and you find yourself migrating multiple modules to stay up to date?

This issue has come up frequently for me in the past year. Given a Github organization wherein 50+ repositories specify some version of a dependency, and the decision is made to standardize that dependency, I wanted an easy way to say "Which repositories do, or do not have this dependency, and at which version?"

So that’s where hasdep comes in.

`npm i -g hasdep`

It's a simple tool you can globally [install from npm](https://www.npmjs.com/package/hasdep), after which you can do things like:

* See which repositories in an organization have a specified dependency
* See which repositories in an organization don't have a specified dependency
* See which repositories in an organization don't have a specified devDependency
* See if a specific repository has a specified dependency
* See which repositories in an organization have a dependency at a desired version

It works with Github.com and internal Github installations (provided your installation has API support enabled).

[Setup, Configuration Options and everything of that nature are on Github](https://github.com/dstevensio/hasdep). If you use the tool and find a problem or wish it did something it doesn't, file an issue and I'll get to work.