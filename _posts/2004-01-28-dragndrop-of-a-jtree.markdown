--- 
layout: post
title: "Drag\xE2\x80\x99n'drop of a JTree"
published: true
meta: {}

tags: 
- Java
type: post
status: publish
---
Did you ever read this [article](http://www.javaworld.com/javaworld/javatips/jw-javatip114.html?) at [JavaWorld](http://javaworld.com)?Its an insanely complex thing it seems to implement drag and drop with Swing. Note, that the behaviour used in this article is the default one used in almost all Windows apps. This article is actually using the old-school DND code, using the awt package.The new jdk1.4 DND code, guess what, doesn't support dropping of folders _between_ other folders. It just supports dropping folders _into_ other folders.Any suggestions here?
