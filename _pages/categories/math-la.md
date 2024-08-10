---
title: "선형대수 (Linear Algebra)"
layout: archive
permalink: categories/math-la
author_profile: true
types: posts
---

{% assign posts = site.categories['math-la']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
