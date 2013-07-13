---
layout: post
title: What happens after the RC2 release
published: true
meta: {}

tags:
- Columba
type: post
status: publish
---
I'm currently discussing things with Timo as we come closer to a final 1.0 release of Columba. First of all there will be another release candidate RC3. This is necessary in our opinion to fix a couple of micro design issues in Columba. Note, that we focus on stuff which could further lead to more work in the future. We fix stuff for the 1.0 release, so that we can add new features afterwards easily. We will get much more attention and users after the stable 1.0 release. So, we don't want to stop contributors adding new stuff, because of problems in our code.Our current planning includes a cleanup of the message viewer component and the message list. Both should be much more generic in our opinion.  For example the message list knows about the fact that it displays email message headers, this way we can't reuse big parts of this swing component. Also on our list is a more generic approach of using the Filters and Filter Actions including the Filter Toolbar and Search Dialog. We would like to move some of the code in the core framework. It currently is a mail-component specific feature. But we want to reuse this code in the addressbook component, too.I'm going to kickstart further discussions in the developer mailinglist. We should really try hard to stay focused on our goals of a quick final 1.0 release.
