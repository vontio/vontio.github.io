---
layout: default
title: vontio
---
{% include JB/setup %}

<div class='post'>
  <h1><a href='{{site.posts.first.url}}'>{{site.posts.first.title}}</a></h1>
  <div class='body'>{{ site.posts.first.content }}</div>
</div>

<ul class="posts">
  {% for post in site.posts offset:1 %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
