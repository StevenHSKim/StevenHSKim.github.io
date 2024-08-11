---
title: "확률및통계 (Probability and Statistic)"
layout: archive
permalink: categories/math-prob
author_profile: true
types: posts
---

{% assign posts = site.categories['math-prob']%}
{% for post in posts %}
  {% include archive-single2.html type=page.entries_layout %}
{% endfor %}
