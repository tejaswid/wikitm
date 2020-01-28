---
title: Miscellaneous Topics
layout: page
---

## Contents

{% for coll in site.collections %}
{% if page.url contains coll.label %}

{% for file in site[coll.label] %}
- [{{ file.title | capitalize }}]({{site.baseurl | append:file.url }})
{% endfor %}
{% endif %}

{% endfor %}
- [How to build a wiki like this?](/misc/howTo/)
- [Broadcasting internet in Windows 10](/misc/broadcastingInternet/)