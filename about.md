---
layout: page
title: About
permalink: /about/
---

{% assign item_list = site.pages | where:"grouped_by","monkey" %}
{% for item in site.collections-dir %}
    * {{ item }}
{% endfor %}

This wiki is a collection of knowledge that we accumulated over the years learning about and working with robots and other cool technologies.

Hope you find it useful.

## Maintainers

[Krishna Manaswi Digumarti](https://krishnamanaswid.github.io)  
[Sundara Tejaswi Digumarti](https://tejaswid.github.io)

We are both post-doctoral researchers in robotics.   
Feel free to checkout our individual webpages to know more about what we do.