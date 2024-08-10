---
title: "미적분 (Calculus)"
layout: archive
permalink: categories/math-cal
author_profile: true
types: posts
---

{% assign posts = site.categories['math-cal']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
