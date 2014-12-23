---
layout: page
title: Salut!
tagline: Welcome to my corner of the Internet
---
{% include JB/setup %}

Blogging is not really my forte so this is my attempt at taking baby steps

#### index of entries

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

