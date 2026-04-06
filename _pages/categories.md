---
title: "Posts by Category"
layout: archive
permalink: /categories/
author_profile: true
header:
  overlay_color: "#4a90e2"
  overlay_image: /assets/images/banner-no-text.svg
---

{% for category in site.categories %}
  {% assign category_name = category[0] %}
  <h3 id="{{ category_name | slugify }}" class="archive__subtitle">{{ category_name }}</h3>
  {% for post in category[1] %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
{% endfor %}
