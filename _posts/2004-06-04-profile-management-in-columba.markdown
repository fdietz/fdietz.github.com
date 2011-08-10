--- 
layout: post
title: Profile Management in Columba
published: true
meta: {}

tags: 
- Columba
type: post
status: publish
---
Many users ask how to start Columba using a different configuration folder.Why not add a profile selection Dialog?This can be achieved pretty easily:- add a "profiles.xml" to the default configuration directory- if Columba is started an no profiles are specified in this file, it will just use the default location- if profiles are specified, a dialog prompts the user for a profile- if Columba is started with the --path option, the user can do this selection  automatically, without the need of the dialog.Example of the profiles.xml:The Profile Management Dialog is simply a list showing all available profiles and buttons to manage them.This way people can easily start Columba with their different configurations including usbsticks, etc.
