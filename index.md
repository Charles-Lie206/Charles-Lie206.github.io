---
layout: archive
title: "Home"
permalink: /
author_profile: true
header:
  overlay_color: "#4a90e2"
  overlay_image: /assets/images/banner-no-text.svg
---

Welcome to my personal blog! 👋

This is my space for recording my technology learning and life.

## 📝 Latest Posts

{% for post in site.posts limit:5 %}
  {% include archive-single.html %}
{% endfor %}

---

[View All Posts]({{ site.baseurl }}/posts/)
