---
title: "컴퓨터비전 (Computer Vision)"
layout: archive
permalink: categories/ai-cv
author_profile: true
types: posts
---

{% assign posts = site.categories['ai-cv']%}
{% for post in posts %}
  {% include archive-single2.html type=page.entries_layout %}
{% endfor %}
