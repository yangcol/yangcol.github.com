---
layout: page
title: yangcol的技术家园
tagline: 热爱技术，热爱生活
---
{% include JB/setup %}

最近的文章

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>