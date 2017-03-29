---
layout: post-talk-jsconf13
title: Brendan Eich - Toward a language-neutral browser VM
youtubeID: O83-d0t0Ldw
---

1. The web as native code platform

AAA games - e.g. halo, etc
Exercise all hardware (memory, gfx etc)

Evolution of web platform - massive improvements in recent years (webRTC,
webaudio, etc)

Emscripten - compile C/C++ -> Javascript

ASM.js - near-native JS perf

Demoed Epic "Sanctuary" - insane that this is JS

ASM approach let's you reuse existing code, not manually rewriting
Mapping to JS

asm - "extraordinarily optimizable" low level subset of JS

not a new language
use of bitwise operators

"use asm" hint
"ahead of time" compilation (versus JIT compilation)

In general, don't write it by hand

2. What about blub?

trans-compilers - good for languages at or near JS's
JS is evolving to fill gaps

open issue - garbage collection
HEAP vs host JS GC
better to hook guest GC to host GC

Lua VM

open issue - JITS
open issue - threads

3. John Henry vs The Steam Hammer
- don't want handcoders who helped advance the language vs VMs racing

WebCL?

Serving two masters. Javascript is in all browsers - keep advancing the
language and its usage

Evolution of language is a good thing, not everything is planned or included
for the reason they are eventually found useful

"Always bet on JS"

Twitter: [@BrendanEich](http://twitter.com/BrendanEich)
