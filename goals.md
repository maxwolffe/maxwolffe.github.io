---
layout: page
title: Goals
---

Deliberate goal setting combined with positive habits. This page tracks my goals for the year / month and where I am with them.

{% for goal in site.categories.goals %}
  [{{ goal.title }}]({{goal.url}})
  {{ goal.excerpt }}
{% endfor %}