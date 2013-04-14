---
layout: post
title: Team Dashboard 2 Release Candidate 2
published: true
categories:
---
This is the second release candidate of [Team Dashboard](https://github.com/fdietz/team_dashboard) and hopefully the last one before the final version 2 release. There are some really exciting new features in this release candidate. So, please give it a try and report issues!

Changelog:
* Number widget optionally displays a metric suffix.
* Number widget supports datapoints data source (number widget now completely replaced the counter widget which was already removed in RC1) as for example graphite (choose `datapoints` as source to view all options)
* New meter widget (using jQuery knob) which uses the number data source.
* New alert widget with its own data source. Initially supports [sensu](http://www.sonian.com/cloud-monitoring-sensu/) (thanks to DraganMileski). Should be useful for future plugins as for example nagios alerts or similar.
* Graph widget supports max y-axis value. Useful if your data has lots of outliers which makes the automatic scaling work nicely again.
* The graph widget has a max y axis value option which is useful if the autoscaling is confused by very large outliers
* Unicorn is the new default rack server. This makes deployment on a free Heroku plan a viable option (#68 - thanks to deathowl)
* Dashboards can be shown in fullscreen mode using [bigscreen.js](https://github.com/bdougherty/BigScreen) (Thanks to ngbroadbent)
* Bugfix: Broken Number::NewRelic source #78
* Bugfix: CI widget loses the light indicator #76
* Bugfix: Sensu bugfixes #72 (thanks to DraganMileski)
* Bugfix: Changing targets length #71 (thanks to hamann)
* Bugfix: Exception Tracker Widget does not work #77
