---
layout: post-talk-jsconf13
title: John-David Dalton - Perf the web forward
category: JSConf US 2013
---

Talking about how to improve existing libraries and your own code with some performance gains techniques. John-David is the creator of [jsperf.com](http://jsperf.com)

Themes
---
optimize for the common case
use natives wisely - array foreach, filter, map, etc vs for loops etc as
alternatives
avoid abstraction - reduce function calls, etc
balance pros/cons

Hoist call & apply
---
(dojo, ember, jquery, prime, underscore would all benefit)
from: while loop { callback.call(values) }
to: callback = createCallback() < - do this outside the loop, createCallback
returns callback.call
then do callback(values) within loop

Avoid binding
---
By detecting "this" use
if not using this, don't bind

Reduce searches
---
indexOf is searching through entire array, large arrays therefore slower
if test on property (obj[key] exists?) that's better but integer 2 == string
"2" so unique operations will fail
cache parts of large arrays in to smaller arrays for searching
(applies to underscore, possibly jquery - these would benefit)

Coerce with care
---
arguments - don't need to slice, can apply it
instead of underscore's flatten (or homegrown) you can concat (native) - use
Array.prototype as the this argument no need to create new

array.push can accept more than one argument

Sugar in moderation
---
chaining API has performance cost in some libs, generic api in others - be
aware of fast/slow paths

lazy.js - <http://dtao.github.io/lazy.js>
very performant in chaining

twitter: [@jdalton](http://twitter.com/jdalton)

www: [jsperf.com](http://jsperf.com)

www: [lodash.com](http://lodash.com) (underscore alternative)

www: [benchmarkjs.com](http://benchmarkjs.com)

hashtag for this talk: [#perffwd](https://twitter.com/search?q=%23perffwd)
