---
layout: default
title: Blog
---

{% include JB/setup %}

{% for post in site.posts limit:3 %}
  <div class="article">
    {{ post.date | date_to_string }}
    <hr>
    <h2><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h2>
    {{ post.content }}
  </div>
{% endfor %}
