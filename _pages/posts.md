---
title: "All Posts"
layout: archive
permalink: /posts/
author_profile: true
header:
  overlay_color: "#4a90e2"
  overlay_image: /assets/images/banner-no-text.svg
---

## 📚 All Posts

{% comment %} Group posts by year {% endcomment %}
{% assign posts_by_year = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}

{% for year_group in posts_by_year %}
  <h2 id="{{ year_group.name }}" class="archive__subtitle">{{ year_group.name }} ({{ year_group.items.size }} posts)</h2>
  
  {% comment %} Group posts by month within the year {% endcomment %}
  {% assign posts_by_month = year_group.items | group_by_exp: "post", "post.date | date: '%B'" %}
  
  {% for month_group in posts_by_month %}
    <h3 id="{{ year_group.name }}-{{ month_group.name | slugify }}" class="archive__subtitle">{{ month_group.name }}</h3>
    {% for post in month_group.items %}
      {% include archive-single.html %}
    {% endfor %}
  {% endfor %}
{% endfor %}
