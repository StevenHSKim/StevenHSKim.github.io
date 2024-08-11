---
title: "etc"
layout: archive
permalink: categories/etc
author_profile: true
types: posts
---

{% assign posts = site.categories['etc']%}
{% for post in posts %}
  {% include archive-single2.html type=page.entries_layout %}
{% endfor %}
