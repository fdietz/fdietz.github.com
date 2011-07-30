--- 
layout: post
title: better win32 mimetype handling, plugin fixes
published: true
meta: {}

tags: 
- Columba
type: post
status: publish
---
Hana commited a patch which uses the win32 registry to determine mimetypes based on file extensions. New code can be found in package org.columba.nativ.mimetype.I've gone through the plugins and fixed compile errors. The python plugins were updated, too.The About Dialog, now also features an Acknowledgement page, listing all third-party libraries we use in Columba.
