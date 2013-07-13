---
layout: post
title: Groovy language plugin support
published: true
meta: {}

tags:
- Columba
type: post
status: publish
---
I'm currently commiting the [Groovy](http://groovy.codehaus.org) language support. A Groovy Interpreter plugin contains all the necessary libraries. It therefore has to be installed first, on order to start writing Groovy plugins. I've also added a simply Hello World Groovy action plugin to demonstrate how to write plugins with Groovy.Note, that have to use the current CVS version, the plugins won't work with RC2.Following the Groovy code which opens a swing dialog:``public class HelloWorldAction extends AbstractColumbaAction {public HelloWorldAction(FrameMediator controller) {super(controller, "Hello, World!")putValue(AbstractColumbaAction.SHORT_DESCRIPTION,                                 "Show me this tooltip, please")}public void actionPerformed(ActionEvent evt) {JOptionPane.showMessageDialog(null, "Hello World!")}}``
