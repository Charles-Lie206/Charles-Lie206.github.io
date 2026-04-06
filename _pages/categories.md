---
title: "Posts by Category"
layout: archive
permalink: /categories/
author_profile: true
header:
  overlay_color: "#4a90e2"
  overlay_image: /assets/images/banner-no-text.svg
---

{% assign categories_max = 5 %}
{% for category in site.categories %}
  {% if category[1].size > 0 %}
    <h2 id="{{ category[0] | slugify }}" class="archive__subtitle">{{ category[0] }}</h2>
    {% for post in category[1] limit: categories_max %}
      {% include archive-single.html %}
    {% endfor %}
    {% if category[1].size > categories_max %}
      <p><a href="{{ site.baseurl }}{{ category[0] }}">View all {{ category[1].size }} posts</a></p>
    {% endif %}
  {% endif %}
{% endfor %}
