--- 
layout: post
title: EasyMock IllegalStateException
published: true
meta: {}

tags: 
- Java
type: post
status: publish
---
I've started using [EasyMock ](http://www.easymock.org/)last year and since then it has become an integral part of my development toolset. I especially love the Java 5 syntax and how self-explaining tests with mock objects are.But today I stumpled over my first [EasyMock ](http://www.easymock.org/)exception which kept me pondering for an hour until I found the solution in another [blog](http://marcels-javanotes.blogspot.com/2007/03/easymock-and-illegalstateexception.html).  The problem was that I was calling a method with an Array of Objects as input parameters.This caused problems because equals of the array failed as expected, but a solution was nicely explained in the docs. Simply use **aryEq()** method, which does the equality check for the objects in the array. Very nice! But, then the **IllegalStateException **caught me. It complained that two matchers were found, but only one recorded. The solution is simple but not really intuitive. Once you starting using matchers you have to use matchers for all arguments of a method call.
