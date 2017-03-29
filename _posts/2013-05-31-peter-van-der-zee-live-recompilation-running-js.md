---
layout: post-talk-jsconf13
title: Peter Van Der Zee - Live Recompilation of Running JS
youtubeID: 9HfWmdp9_I0
---

compile JS on the fly
maintains access to closures
no restart
lazy evaluation
generic approach - works with pretty much any js

compile in JS

    function getFunction () {
      return function () {
        return Function ('string rep of function')();
      }
    }

Issues
---
- function argumnets
- closures
- func declarations
- named function expressions
- performance

Direct vs indirect eval
- direct (eval) has access to lexical scopes
- indirect (Function) only global

Open issues
---

Big
-Inserting new functions

Minor
-variable clashes
-hard to explain

Only for code that will be reinvoked at some point (like callbacks)

<http://github.com/qfox/recompiler>

www: [qfox.nl](http://qfox.nl)

