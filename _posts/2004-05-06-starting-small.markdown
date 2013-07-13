---
layout: post
title: "Starting small\xE2\x80\xA6"
published: true
meta: {}

tags:
- Columba
type: post
status: publish
---
Its raining outside, a good day to fix some smaller issues in Columba.First of all I externalized all pending strings in the Account Dialog most notably the Spam Filter Options Panel, where I also fixed the layout.Afterwards I've assigned every modal JDialog a correct parent JFrame, instead of just passing null. I hope this fixes the focus/window handling users noticed, as reported in the [bugtracker](http://sourceforge.net/tracker/index.php?func=detail&aid=866186&group_id=21217&atid=121217).Additionally, all plugins now compile cleanly. Use and the buid.xml script in the plugins directory to build all plugins. This script stops execution if a build fails. The buildall.sh shell script skips compile errors, so you can make sure that all working plugins compile.
