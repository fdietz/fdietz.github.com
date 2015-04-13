---
layout: post
title: "Team Dashboard v0.4 released"
published: true
meta: {}
tags: []

type: post
status: published
---

This was a busy week, lots of new stuff to check out!

Most notable changes are:

* Graphite Function Editing made easy with a built-in function browser (including reference documentation) and improved error reporting.
* HTTP Proxy Sources also got a preview mode where you can see the JSON response and can easily select a specific attribute from it.
* All source plugins can easily specify their own configuration params. These will automatically appear in the widget editor dialog and are persisted as part of the widget configuration. The existing internal sources are already using it. Documentation can be found in the [README](https://github.com/fdietz/team_dashboard).

Apart from that we fixed a nasty bug in the Ganglia source ([Issue](https://github.com/fdietz/team_dashboard/issues/15)), another one in the Graphite source (you can now again fetch multiple targets [Issue](https://github.com/fdietz/team_dashboard/issues/4)), updated to the latest Twitter Bootstrap version and documented Team Dashboard's own [REST API](https://github.com/fdietz/team_dashboard).

_Warning:_ This release might break some widget configuration due to the changes in the source plugin configuration. Sorry for any inconveniences!

So, go ahead and git pull to the latest version!

[Milestone v0.4 Issues](https://github.com/fdietz/team_dashboard/issues?milestone=2&state=closed)

More information on how to get started on [github](https://github.com/fdietz/team_dashboard).

Thanks go to [XING](http://www.xing.com) for sponsoring this weeks development on Team Dashboard
