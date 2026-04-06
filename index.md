---
layout: single
title: "Home"
permalink: /
author_profile: false
header:
  overlay_color: "#4a90e2"
  overlay_image: /assets/images/banner-no-text.svg
---

<div style="text-align: center; margin-bottom: 40px;">
  <img src="/assets/images/bio-photo.svg" alt="Charles" style="width: 150px; height: 150px; border-radius: 50%; margin-bottom: 20px;">
  <h2 style="margin: 0;">Charles</h2>
  <p style="margin: 10px 0; color: #6c757d;">Tech Blog | Recording Learning and Growth</p>
</div>

Welcome to my personal blog! 👋

Hi, I'm **Charles** — a tech enthusiast sharing knowledge and insights about programming, networking, and system administration. This is my space for documenting my learning journey and helping others along the way.

## 📝 Latest Posts

{% for post in site.posts limit:5 %}
  {% include archive-single.html %}
{% endfor %}

---

[View All Posts]({{ site.baseurl }}/posts/)
