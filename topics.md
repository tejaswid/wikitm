---
layout: page
title: Topics Home
permalink: /topics/
---

## Contents

{% assign ordered_collections = site.collections | sort: 'sequence' %}
{% for coll in ordered_collections %}
    {% comment %} Skip Posts {% endcomment %}
    {% if coll.label == "posts" %} 
        {% continue %} 
    {% endif %}
  
    {% comment %} Link to the hompage of the topic {% endcomment %}
    <!-- <a href={{ site.baseurl | append:"/" | append:coll.label | append:"/" }}>{{ coll.menu_heading }}</a> -->
    [{{ coll.menu_heading }}]({{ site.baseurl | append:"/" | append:coll.label | append:"/" }})
    {% for file in site[coll.label] %}
        {% if file.name contains "index" %}
      	    {% continue %}
        {% endif %}
        - [{{ file.title | capitalize }}]({{site.baseurl|append:file.url}})
    {% endfor %}
{% endfor %}