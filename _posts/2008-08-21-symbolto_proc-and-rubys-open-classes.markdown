---
layout: post
title: Symbol#to_proc and Ruby's Open Classes
published: true
meta:
  _edit_lock: "1219327582"
  _edit_last: "1573847"
tags:
- Rails
- Ruby
type: post
status: publish
---
Just saw an [article](http://www.infoq.com/articles/ruby-open-classes-monkeypatching) related to my [last post](http://fdietz.wordpress.com/2008/08/12/symbolto_proc-what/) about Symbol#to_proc. Very interesting read!

And even more fun is this article which explains the same mechanism and [currying](http://www.infoq.com/news/2008/02/to_proc-currying-ruby19) on top:`proc {|x, y, z| x + y + z }.curry` returns the equivalent of: `proc {|x| proc {|y| proc {|z| x + y + z } } }`

Here's the example: `plus_five = proc { |x,y,z| x + y + z }.curry.call(2).call(3)plus_five[10]  #=> 15` I've seen something similar in [Scala](http://www.scala-lang.org/).

A good explanation of Scala function currying can be found in [Daniel Spiewak's blog](http://www.codecommit.com/blog/scala/function-currying-in-scala). BTW, Scala finally got a new website!

And even better: Just got an E-Mail from [Artima.com](http://www.artima.com/) that the 4th edition of "[Programming in Scala](http://www.artima.com/shop/programming_in_scala)" arrived. Something to read for the weekend!
