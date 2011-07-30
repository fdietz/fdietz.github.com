--- 
layout: post
title: A more-failsafe approach of handling configuration and headercache loading/saving
published: true
meta: {}

tags: 
- Columba
type: post
status: publish
---
We've had some interesting problems with corrupt configuration or headercache files, which is always annoying for users.Following the proposal to fix this particular issue. The proposed behaviour can be found in other applications pretty often, too.Saving files:- save the configuration/headercache file to "blabla.new"- if success:  - rename the old file to "blabla.old"  - rename "blabla.new" to "blabla"Loading files:- try to load "blabla"- if the file is corrupt:  - delete "blabla"  - open "blabla.old" instead (fall-back to the last working configuration)
