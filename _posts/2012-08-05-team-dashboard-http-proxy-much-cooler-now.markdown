---
layout: post
title: "Team Dashboard HTTP Proxy support much cooler now"
published: true
meta: {}
tags: []

type: post
status: published
---

Team Dashboard Number and Boolean Widgets support a HTTP Proxy data source already. Idea is to give it a URL of an existing JSON service which follows the format as specified in the documentation. It will then extract the current value from the JSON structure for you.

But what if you have an existing service with a different structure? With the new version you can additionally set a value path using a dot notation to select a specific value in your JSON structure.

Example JSON:
{% raw %}
{
  "parent" : {
    "child" : {
      "child2" : "myValue"
    }
  }
}
{% endraw %}

A value path of "parent.child.child2" would resolve "myValue".
