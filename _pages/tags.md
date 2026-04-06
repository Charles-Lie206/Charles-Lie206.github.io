---
title: "Posts by Tag"
layout: archive
permalink: /tags/
author_profile: true
header:
  overlay_color: "#4a90e2"
  overlay_image: /assets/images/banner-no-text.svg
---

{% for tag in site.tags %}
  {% assign tag_name = tag[0] %}
  <h3 id="{{ tag_name | slugify }}" class="archive__subtitle">{{ tag_name }}</h3>
  {% for post in tag[1] %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
