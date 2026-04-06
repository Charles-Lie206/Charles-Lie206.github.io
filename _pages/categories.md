---
title: "Posts by Category"
layout: archive
permalink: /categories/
author_profile: true
header:
  overlay_color: "#4a90e2"
  overlay_image: /assets/images/banner-no-text.svg
---

{% comment %}Collect all unique categories{% endcomment %}
{% assign unique_categories = '' | split: '' %}
{% for post in site.posts %}
  {% for category in post.categories %}
    {% unless unique_categories contains category %}
      {% assign unique_categories = unique_categories | push: category %}
    {% endunless %}
  {% endfor %}
{% endfor %}

{% comment %}Display posts for each category{% endcomment %}
{% for category in unique_categories %}
  <h3 id="{{ category | slugify }}" class="archive__subtitle">{{ category }}</h3>
  {% for post in site.posts %}
    {% if post.categories contains category %}
      {% include archive-single.html type="grid" %}
    {% endif %}
  {% endfor %}
{% endfor %}
