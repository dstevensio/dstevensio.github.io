---
layout: post-talk-jsconf13
title: Filip Maj & Michael Brooks - Master the Command Line with Node.js
category: JSConf US 2013
---

patterns to follow emerged from concepts discovered while building tools
(learned the hard way) - phonegap

Tools used by people and some of them are used by other tools to delegate
responsibility

Found CLI qualities that are useful...

will be sharing 4 & explaining how to implement them in node
suggestions not requirements - this is how they did it

A command-line interface should be testable
---
node community very diligent for testing modules
rarely do this with CLI stuff we release
have struggled to test the CLI
originally tried testing by shell execution - really slow, results were vague
this testing susceptible to false positives

We want to programmatically test CLI
this can be done by treating the CLI as a module
slim down the bin/ script to proxy into the CLI module - treat it as an
interfact to the module

instead of all the bin script being in that file (e.g. bin/jsconf.js)
have that file just require a lib/cli/x.js file and the bin script initiates it

extract functionality in to different files, test CLI as a module as a result,
because you can fake process.argv so you can programatically send in arguments
as if they'd been entered on the commandline

CLI module should ONLY have CLI functionality, keep it clean
net result: testable CLI without doing anything especially creative

CLI should be helpful
---
People are using your tool, so they will almost certainly need help to make
things work.·

Meaningful help output to assist people is extremely important

It's convenient to hard-code the helpful messages in the logic - big ugly
strings buried in code- hard to maintain/find/contribute to - so far from ideal
if you're hoping to keep the help messages helpful

easier way to document - and it's obvious...
use text files that map to specific commands or general usage - in a doc/
folder for instance, cli folder, help.METHOD.txt content of text files is
familiar to any --help output you've seen in \*nix
Easy to find and show relevant help when needed.

if arguments are cli-executable help file create, then this results in reading
a file at doc/cli/help/file/create.txt or doc/cli/help/file.create.txt, etc
(based on personal style/preference)

Use templating and markdown libraries when plain text isn't good enough.

A CLI should be bipolar
---
Sometimes it's nice to have verbose tool when things go wrong
Other times (most of the time) it's nice for the tool to be quiet when things
go right

But how to implement this? event emitters work well. Main lib is chatty - tells
what's doing all over. But you don't have to listen to it - you can hook up a
listener and console.log it if requested/it makes sense, you can add levels so
you can listen just to the ones you care about, or not listen at all.

e.g. only want it to console.log stuff if things go wrong? listen to 'error'
level messages / listen to all if you want it to be super verbose, don't listen
if you want it to be quiet, etc.

Therefore you have detailed messages you can turn on or off wherever you're
consuming the CLI

CLI should be interoperable
---
important to be nice to others / play nice to others. Not uncommon for tools to use tools to build
better tools. Only way for this to really work is with good communication
between tools and conventions. Exit codes are important - don't ignore them.

process.exit(0) - 0 means successful completion (usually)
process.exit(1) etc

allow output format to be specified - e.g. your messages are plain text or JSON
or can be better formatted to return certain information only, etc - so others
can work with your tool.

Try to avoid hard-coding paths into your code - like using './' in paths -
kills windows. Use path.join() - standard lib - which will handle this for you

npm is windows-friendly so scripts add binaries to PATH - from package.json
when you npm install (only binaries) so you don't need paths, which are tricky
as different on windows / \*nix

Slides: <http://michaelbrooks.ca/deck/jsconf2013>

Twitter: [@filmaj](http://twitter.com/filmaj)

Twitter: [@mwbrooks](http://twitter.com/mwbrooks)
