--- 
layout: post
title: Plugins must specify src/ directory
published: true
meta: {}

tags: 
- Columba
type: post
status: publish
---
Plugins now have to additionally specify all directories containing sourcecode. Same goes for resource files.As always all changes have to be made in **build.properties**.Building of all plugins can now be started by simply calling "``ant plugins``".
