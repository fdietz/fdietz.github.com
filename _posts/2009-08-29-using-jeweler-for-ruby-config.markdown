--- 
layout: post
title: Using jeweler for ruby-config
published: true
meta: 
  _edit_lock: "1252165461"
  _edit_last: "1573847"
tags: 
- Ruby
type: post
status: publish
---
I've been using [jeweler](http://github.com/technicalpickles/jeweler/tree/master) for ruby-config and am very surprised how smooth everything went. What I especially like about jeweler is that its absolutely a no-brainer to create gems and post them via github. And with the versioning support its a joy to push out new releases! Am probably going to move over my hoe based projects to jeweler now.So, in order to get started with jeweler you have to install the jeweler gem first:``gem install technicalpickles-jeweler``Next to bootstrap a new project you do:``jeweler --create-repo --summary "Ruby-Config" ruby-config``This will create a project skeleton with all the usual candidates. What's left to do now is edit the Rakefile to setup the gem configuration and adding some sourcecode. Don't forget the tests!Now, you simply create an initial version with:``rake version:write MAJOR=0 MINOR=1 PATCH=0``which update the VERSION file in your project for you.When you do a "rake release", jeweler will create a new gemspec file for you, commit and push the update, create a version tag and push it, too.When simply updating a minor feature, I do a "rake version:bump:minor" and afterwards a "rake release" and I'm done. Great stuff!
