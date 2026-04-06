---
title: "Posts by Tag"
layout: archive
permalink: /tags/
author_profile: true
header:
  overlay_color: "#4a90e2"
  overlay_image: /assets/images/banner-no-text.svg
---

{% assign posts_by_tag = site.tags | sort %}
{% for tag in posts_by_tag %}
  <h2 id="{{ tag[0] | slugify }}" class="archive__subtitle">{{ tag[0] }}</h2>
  {% for post in tag[1] %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
