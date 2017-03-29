---
layout: post-talk-jsconf13
title: John Kleinschmidt - Surviving the Offline Apocalypse
youtubeID: Qg75x08Mtcs
---

Offline matters:
Hospital/remote/etc for US
Globally - much more important (Africa, other 3rd world etc - connectivity 10
yrs behind US - spotty at times, could be mobile)
Your server(s) could be unavailable

"Offline first" design - assume you're offline
make sure functionality works while offline

Live off the grid - Application Cache
--
- it's kind of a jerk (But it works well)
cache manifest
CACHE - files required for offline usage (browser will download and cache)
NETWORK - can be fetched when needed (can be wildcard \*)

note - CACHE - these will be served from cache even if online so make part of
build step to update the manifest to give latest version

Good browser support - forget about IE < 10 and Opera Mini

Supplies: IndexedDB
---

object store - key/value pairs - noSQL DB
- heavily Async
- versioning (window.indexedDB.open('databasename', VERSION\_NUMBER)) ->
  request.onupgradeneeded

Chrome Dev Tools give good insight in to the indexedDBs you create
Browser support - not perfect (iOS safari doesn't though) - polyfill available
that covers all but opera mini. Native support by Firefox & Chrome, plus IE 10+

Adequate Weaponry: Filesystem API
---

- sandboxed files stored for your app only
- provides local url to that file
- persistent (until deleted) or temporary (lasts for the session)
- must request access
- async API

Browser support: Chrome only, polyfill available for IndexedDB (extends support
to those who support IndexedDB)

Escape Plan
---

navigator.onLine ("might" be online = true, definitely offline = false)
XHR to see if actually able to reach anything
window.on('online') - hey we're online, window.on('offline') - oh we're
offline, XHR to do an action - if fails, we're offline, etc.

Cure already saved over 9000 kids with their offline app - awesome!

slides: [jkleinsc.github.io/jsconf13](http://jkleinsc.github.io/jsconf13)

twitter [@jkleinsc](http://twitter.com/jkleinsc)

www: [cure.org](http://cure.org)

blog: [resplendentdev.com](http://resplendentdev.com)
