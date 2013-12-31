---
layout: post
title: "Team Dashboard supports Ganglia"
published: true
meta: {}
tags: []

type: post
status: published
---

Team Dashboard adds a Ganglia data source to show your system metrics as graph or counter widgets.

In order to get started you have to configure the URL to your Ganglia installation. The easiest way is to use the environment variable GANGLIA_URL.

For example:
{% raw %}
GANGLIA_URL=http://ganglia-host rails server
{% endraw %}


More information on how to get started on [github](https://github.com/fdietz/team_dashboard).
