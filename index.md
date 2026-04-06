---
layout: single
title: "Home"
permalink: /
author_profile: false
header:
  overlay_color: "#4a90e2"
  overlay_image: /assets/images/banner-no-text.svg
---

Welcome to my personal blog! 👋

Hi, I'm **Charles** — a tech enthusiast sharing knowledge and insights about programming, networking, and system administration. This is my space for documenting my learning journey and helping others along the way.

## 📝 Latest Posts

{% for post in site.posts limit:5 %}
  {% include archive-single.html %}
{% endfor %}

---

[View All Posts]({{ site.baseurl }}/posts/)
