---
layout: page
title: Blog
---

I occasionally have thoughts which are worth sharing.

{% for post in site.categories.blog %}
  [{{ post.title }}]({{post.url}}) - {{post.date_updated}}
  {{ post.excerpt }}
{% endfor %}