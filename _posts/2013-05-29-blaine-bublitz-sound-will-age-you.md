---
layout: post-talk-jsconf13
title: Blaine Bublitz - Sound Will Age You
---

"Sounds are important" - TV/games with sound off, not as good

"Sounds are iconic" - examples of mario, sonic, pacman sounds, everyone
recognizes them

"Sounds are hard" - on and off the web

FrozenJS - game development is his perspective

- WebAudio vs HTML5 Audio
- Codecs
- Mobile

started with just webaudio, no fallbacks

Web Audio:
* high performance
* low latency
* buffers not resource loading

Then do fallback:

get all audio types, sort by probably / maybe / drop the no response (using
canPlay type) and try most likely to least likely, exit once one starts
playing.

on mobile, autoplay/load disabled (data usage cited as reason)
attach to touchstart and load/play there (for each audio element)

Chrome flag for android chrome : "Disable gesture requirement for audio load"
wants everyone to enable this

Audio streams
- layering of sounds

- on mobile? single stream only. play one, any other played will cut it off and
  play
chrome flag for android chrome - enable webaudio android (wants you to enable
this)

Summary:
start at web audio -> fallback to HTML5 -> don't fallback to flash -> push web
audio (enable in chrome://flags in Chrome Beta for Android)

Need to improvoe:
Mobile, canPlayType & string comparison, better abstractions & fallbacks

Howler.js, webaudio.js, etc

Game sounds with fallbacks:
Frozen, SoundJS, Quintus, Audia

Twitter: [@BlaineBublitz](http://twitter.com/blainebublitz)

Slides: [blog.iceddev.com/sound-will-age-you](http://blog.iceddev.com/sound-will-age-you)
