---
title: "All Posts"
layout: archive
permalink: /posts/
author_profile: true
header:
  overlay_color: "#4a90e2"
  overlay_image: /assets/images/banner-no-text.svg
---

{% comment %} Collect all unique years and months {% endcomment %}
{% assign unique_years = '' | split: '' %}
{% for post in site.posts %}
  {% assign post_year = post.date | date: '%Y' %}
  {% unless unique_years contains post_year %}
    {% assign unique_years = unique_years | push: post_year %}
  {% endunless %}
{% endfor %}

{% comment %} Sort years in descending order {% endcomment %}
{% assign sorted_years = unique_years | sort | reverse %}

{% comment %} Display posts for each year {% endcomment %}
{% for year in sorted_years %}
  <h3 id="{{ year }}" class="archive__subtitle">{{ year }} — 
    {% assign year_count = 0 %}
    {% for post in site.posts %}
      {% if post.date | date: '%Y' == year %}
        {% assign year_count = year_count | plus: 1 %}
      {% endif %}
    {% endfor %}
    {{ year_count }} posts
  </h3>

  {% comment %} Collect unique months for this year {% endcomment %}
  {% assign unique_months = '' | split: '' %}
  {% for post in site.posts %}
    {% if post.date | date: '%Y' == year %}
      {% assign post_month = post.date | date: '%B' %}
      {% unless unique_months contains post_month %}
        {% assign unique_months = unique_months | push: post_month %}
      {% endunless %}
    {% endif %}
  {% endfor %}

  {% comment %} Display posts for each month {% endcomment %}
  {% for month in unique_months %}
    <h4 id="{{ year }}-{{ month | slugify }}" class="archive__subtitle">{{ month }}</h4>
    {% for post in site.posts %}
      {% if post.date | date: '%Y' == year and post.date | date: '%B' == month %}
        {% include archive-single.html %}
      {% endif %}
    {% endfor %}
  {% endfor %}
{% endfor %}
