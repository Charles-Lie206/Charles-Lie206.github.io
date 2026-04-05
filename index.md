---
layout: archive
title: "首页"
permalink: /
author_profile: true
header:
  overlay_color: "#2c3e50"
  overlay_image: /assets/images/placeholder.svg
  caption: "技术博客 | 记录学习与成长"
---

欢迎来到我的个人博客！👋

这里是我在技术学习和生活中的记录空间。

## 📝 最新文章

{% include base_path %}
{% for post in site.posts limit:5 %}
  {% include archive-single.html %}
{% endfor %}

---

[查看所有文章]({{ site.baseurl }}/posts/)
