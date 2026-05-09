---
layout: page
title: HCI Project Diary
---

# HCI Project Diary

This page lists my HCI project entries.

{% for post in site.posts %}
- [{{ post.title }}]({{ post.url | relative_url }}) — {{ post.date | date: "%B %d, %Y" }}
{% endfor %}
