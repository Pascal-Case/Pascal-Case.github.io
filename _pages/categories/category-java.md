---
title: "java"
layout: archive
permalink: categories/java
author_profile: true
types: posts
---

{% assign posts = site.categories['java']%}
{% for post in posts %}
{% include archive-single.html type=page.entries_layout %}
{% endfor %}
