---
layout: page
title: Tags
permalink: /tags/
---

<ul>
  {% for tag in site.tags %}
    <li><a href="#{{ tag[0] | slugify }}">{{ tag[0] }}</a> ({{ tag[1].size }})</li>
  {% endfor %}
</ul>

{% for tag in site.tags %}
  <h3 id="{{ tag[0] | slugify }}">{{ tag[0] }}</h3>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}