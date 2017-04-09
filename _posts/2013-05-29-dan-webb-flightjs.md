---
layout: post-talk-jsconf13
title: Dan Webb - flight.js
category: JSConf US 2013
---

aim: decouple components
- components have a simple contract with the outside world
- lets you refactor a bit at a time rather than having dependencies that mean
  refactoring is a large task

How?
- make greater use of DOM
|- object w/ reference to DOM node, can manipulate the DOM node, trigger/listen to events, etc

App = collection of components working in isolation

github: twitter/flight-jasmine - test components w/ jasmine

- not equivalent to frameworks like angular, etc
- doesn't dictate architecture

github: [github.com/twitter/flight](https://github.com/twitter/flight)

twitter: [@flight](http://twitter.com/flight) and [@danwrong](http://twitter.com/danwrong)
