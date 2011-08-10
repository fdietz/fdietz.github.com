--- 
layout: post
title: Spamming techniques
published: true
meta: {}

tags: 
- Columba
- Java
type: post
status: publish
---
As I'm currently working on finishing my dissertation, I stumpled upon some very interesting spam message recently.I'm writing about macchiato, a spam filter library, written in Java, where I'm using a naive bayesian filter to detect spam. Its an adaptive filter which gets trained by the user's messages. As I'm close to finish this thing, I had to draw a line where I won't go further optimizing the library - at least as part of my dissertation. Development will of course continue - Milestone M3 of [Columba](http://columba.sourceforge.net) will have this integrated.Now to my recent discoveries in the faszinating world of penis enlargements, horny women and nigerian millionars...Most spam message tend to be written using HTML. Now take a look at the word Free in html:``Fr&lt;font size=0&gt;&amp;#38nbsp;&lt;/font&gt;ee``Did you see the trick? **&amp;#38nbsp;** is actually just a space, but it has a zero font size, so it looks correct to the user, but my text tokenizer won't recognize this word. It will recognize **Fr** and **ee** as independent tokens, though. Which makes no sense :-(I'll have to further improve the message tokenizer to be more aware of html than it is currently.
