---
title: Miscellaneous Topics
layout: page
---

## Contents

{% for coll in site.collections %}
{% if page.url contains coll.label %}

{% for file in site[coll.label] %}
{{file.name}}
{% if file.name contains "index" %}
	{% continue %}
{% endif %}

- [{{ file.title | capitalize }}]({{site.baseurl|append:file.url}})

{% endfor %}

{% endif %}
{% endfor %}
