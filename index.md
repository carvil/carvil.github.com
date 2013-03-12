---
layout: default
title: Blog
---

{% include JB/analytics %}
{% include JB/setup %}

<a href="https://github.com/carvil"><img style="position: absolute; top: 0; right: 0; border: 0;" src="https://s3.amazonaws.com/github/ribbons/forkme_right_darkblue_121621.png" alt="Fork me on GitHub"></a>

{% for post in site.posts limit:3 %}
  <div class="article">
    {{ post.date | date_to_string }}
    <hr>
    <h3><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h3>
    {{ post.content }}
  </div>
{% endfor %}
