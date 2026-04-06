---
title: "Posts by Category"
layout: archive
permalink: /categories/
author_profile: true
header:
  overlay_color: "#4a90e2"
  overlay_image: /assets/images/banner-no-text.svg
---

{% assign posts_by_category = site.posts | group_by_exp: "post", "post.categories | sort: natural" %}
{% for category in posts_by_category %}
  <h2 id="{{ category.name | slugify }}" class="archive__subtitle">{{ category.name }}</h2>
  {% for post in category.items %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
