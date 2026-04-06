---
title: "All Posts"
layout: archive
permalink: /posts/
author_profile: true
header:
  overlay_color: "#4a90e2"
  overlay_image: /assets/images/banner-no-text.svg
---

{% assign posts_by_year = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
{% for year_group in posts_by_year %}
  {{ year_group.name }} — {{ year_group.items.size }} posts
  {% assign posts_by_month = year_group.items | group_by_exp: "post", "post.date | date: '%B'" %}
  {% for month_group in posts_by_month %}
    {{ month_group.name }}
    {% for post in month_group.items %}
      {% include archive-single.html %}
    {% endfor %}
  {% endfor %}
{% endfor %}
