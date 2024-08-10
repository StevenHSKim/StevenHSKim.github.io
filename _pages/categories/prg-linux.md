---
title: "리눅스 (Linux)"
layout: archive
permalink: categories/prg-linux
author_profile: true
types: posts
---

{% assign posts = site.categories['prg-linux']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
