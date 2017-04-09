---
layout: post-talk-jsconf13
title: Felix Geisendorfer - tus.io - Squeezing Cats Through Tiny Mobile Tubes
category: JSConf US 2013
---

File uploads
1971 - FTP
Predates TCP/IP by 3 years

1995- php, IE, JS, HTML2, form based file uploads

RFC1867 form upload

Complexity was perfect level but features were lacking (progress indication etc)

Flash started being used because of progress indicators :(

2008 - XHR Level 2
(Ajax unable to do file uploads before this)

2009 - file API

2013 - no resumability. Today file
Sizes are huge.

How to resume http upload?
Rfc 2616

Range requests
- can be used for resumable uploads

S3 proprietary - not ideal
Bad for open web

resumable.js - splits file in to fixed size chunks
Best available open source solution
Not defined as a standard

Vision: low complexity to add resumable file uploads

Enter: tus.io
Informal working group
Open to all to join
Goal to publish as RFC

Content-Length and Final-Length http headers on POST request - tells server we intend to upload file of size final-length

PATCH http header
Upload whole file, 200 returned if successful, if not do a HEAD against file we created and see content-length

brewtus - node server for this

Also: upload acceleration
Eg parallel requests/connections per upload etc

Twitter: [@felixge](http://twitter.com/felixge)

www: [tus.io](http://tus.io)
