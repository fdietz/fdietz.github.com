--- 
layout: post
title: online/offline support, more bugfixes
published: true
meta: 
  _edit_last: "1573847"
  _edit_lock: "1251238348"
tags: 
- Columba
type: post
status: publish
---
I've started making use of the new online/offline connection status.When downloading or sending messages, Columba automatically switches to online state. If in offline status, Columba won't automatically check for new messages.Fixed bugs:-  #966413: mimetype viewer doesn't remember apps-  fixed classcast exception, actions are enabled/disabled correctly again in addressbook-  group dialog didn't show up in addressbook-  #968740, can't choose folder in filter dialog, if treemodel is not sorted
