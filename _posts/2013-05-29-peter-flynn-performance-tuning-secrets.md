---
layout: post-talk-jsconf13
title: Peter Flynn - Performance Tuning Secrets
youtubeID: 480JL_WuMt0
---

Peter Flynn (Adobe)

performance.now()

everything after the initial execution matters - CSS, repaint, etc

Rendering
---

\*- Dev tools timeline tricks
console.time() < - custom timeline marker
Remote debugging API, JSON API exposed over a socket connection
Telemetry testing framework on top of debugging API (Google)
Includes UI automation - scrolling API enabled to simulate scrolling accurately

page.json -> controller for automation

topcoat project (open source css framework) -> good example of telemetry usage
github: topcoat/topcoat-server
FPS meter / continuous page repainting are useful (google this)
show paint rectangles & layer borders - show repainting

\*- chrome://tracing
close all tabs except one you're testing
alt + mousewheel zooms
A + D - pan horizontally
W + Z - zoom in/out

600FPS camera filming actions to measure before FPS meter was available!

VM Guts (V8)
---

Memory profiling
CPU profiling
Instrumented CPU Profiling - console.profile(name) console.profileEnd(name)
console.time('label'); console.timeEnd('label');

V8 Logging Analysis
plot-timer-events -> close all chrome insts. launch chrome with flags, run
scripts on log produced, get PNG file graph.

code kind - optimized? Not optimized? obv. want optimized code running
--range=start,end to filter down
Tick processor - * = optimized no * = not optimized

opt/deopt tracing
- see where unoptimized code is running and why optimization was disabled

chrome --no-sandbox --js-flags="--trace-opt-verbose --trace-deopt"
"file://foo.html" > opt-log.txt

Things that might be more important than JS perf optimizing:
1. Network IO performance is very important
- performance.timing;
- performance.webkitGetEntriesByType('resource');
- appearance of speed is often just as good as actual speed (async processes
  meaning the action doesn't block while it completes gives impression of being
  fast even if it's not that fast)

2. Code might be good enough already
- If the actual impact of your less than optimal performance is not perceivably
  worse, no point in killing yourself to get 80m ops per second versus 12m ops
  per second when 12m ops per second is faster than can be perceived by user,
  move on!

3. Better use of time?
- code cleanup / fixing bugs / documenting / etc

Slides: [https://github.com/peterflynn/jsconf-2013](https://github.com/peterflynn/jsconf-2013)
