---
layout: post
title: UX for the Engineers
category: Non-customer facing UX
---


Firstly, this was a tricky one to title. To clear up any confusion, and perhaps save you some time, this is not about helping engineers understand User Experience (UX).

<strong>This piece is about spending a little time on your developer-facing software to make it friendly to the end-user: the engineer.</strong>

Through necessity, and indeed through the research-backed successes that result, it's no surprise that we take great care when developing software for an end-user. Often an end-user who may not be especially tech-savvy, and in most cases, not as tech-savvy as the person or persons responsible for creating the software.

We have entire departments (or attribute one of the many hats a startup employee wears) dedicated to User Experience, and they focus on what the customer will respond best to, make easiest work of, and enjoy most.

But what about that forgotten customer: other developers consuming the libraries and shared components, modules and related software that we are writing?

I recently came to find myself writing a development server that could be slotted in to an application, removing the need for every engineer using the wider ecosystem to duplicate the work of setting up a basic server to bootstrap a project. While I attempted to use sensible defaults wherever possible, there was some unavoidable configuration required by a consumer of this server; And therein, the chance that such configuration could be forgotten. An error in this instance would be the right thing to occur, and so an error was generated.

```
index.js:24
  throw err;
  ^
Error: Could not find config
  at ...
```

Bear in mind that this module was going to be required by multiple applications, among many other dependencies. As a developer, when that error pops up in your console, you start that delightful task we all know so well, of traversing through the stack trace to work out where it's being thrown and any other relevant details that might help you track down the cause.

When this appeared for me [yes — I managed to run it once during development having forgotten to include a configuration file that I myself had made mandatory...] it struck me that this was a really poor experience.

Here I was, about to release a module for internal use with the stated aim of making engineers lives easier, and a simple missed step in setup results in an unhelpful and very abrupt halt.

So, I resolved to do something about it. First, there are many levels to a modern web application and stack traces alone can be a less than perfect solution to tracking down where the error is even being thrown. To combat this, the first step was to begin each error with a clear, unmissable callout:

```
This Error is thrown by the node-server module.
-----------------------------------------------
```

Here, node-server is the name that appears in the package.json for this project. Make it easy on the developer; give them a name they can pick out from their dependencies.

Next, give prominence to the human-readable error message. If your application doesn't throw human-readable error messages, fix that first!

`Missing config file in config/node-server/server.js`

Chances are, you also know the likely resolution steps of common errors like these. So why not communicate the appropriate resolution right here in the error message?

```
Please create the config file at the expected path. Refer to the Github repository blah/node-server for format and configuration options.
```

But we'd never assume that this is enough alone — perhaps it's not as simple as the expected resolution, or maybe it looks like an error you've seen before, but really something else is causing it to happen. So, give clear instructions on how to escalate the issue if that resolution fails:

```
If you have followed this resolution step and you are still seeing an error, please file an issue on the node-server repository:
_URL_TO_REPOSITORY_ISSUES_PAGE_HERE_
```

Then, finish everything up by not hiding any of the useful information that the original error provided — drop the full stack trace last:

```
/Users/dstevensio/ws/node-server/lib/config-loader:58
  throw serverConfigError
```

Naming your error references contextually also helps — here, when creating a callback for server startup, I've used `serverConfigError` rather than just `err`, so that the stack trace output is more meaningful.

If you're working in node.js there is the [Chalk](https://www.npmjs.com/package/chalk) module that makes coloring your terminal output simple, outside of node I'm sure tracking down a suitable module or library will be easy, and using appropriate colors further improves your consuming developer's experience. Check out this screenshot for an example:

![Action Screenshot](/images/errorexample.png)

Taking this approach, you can head off some frustrating experiences and endear yourself to the folks who actually use the software you create. Who knows, maybe someone will do this for software you consume yourself, and you get to benefit as well...