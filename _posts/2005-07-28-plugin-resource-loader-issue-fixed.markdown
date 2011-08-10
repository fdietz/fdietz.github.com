--- 
layout: post
title: Plugin resource loader issue fixed
published: true
meta: {}

tags: 
- Columba
type: post
status: publish
---
I've just fixed the plugin resource loading bug. Loading of images or xml-files or whatsoever didn't work.Again, you have to specify your plugin resource folder in build.properties. Then to load the resource  simply do a "``this.getClass().getResourceAsStream(String resource)``".I've had to change a couple of methods in core to make this work. All these methods didn't use **InputStream **as the parameter to pass resource file content. Instead they used the path to the resource as String. This is wrong from an API user's point of view, because the internal loader doesn't know where these resources come from. You might even want to load resources from a webserver. So, simply using **InputStream **makes the API much more decoupled from the internal implementation.
