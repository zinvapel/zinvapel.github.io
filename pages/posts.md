---
layout: page
title: Список постов
permalink: /posts
category: about
date: 2017-10-25 21:08:00+03:00
ishead: true
---

<table style="border-collapse: collapse;border: none;">
{% for post in site.posts %}
  <tr>
    <td><a href="{{ post.url | absolute_url }}">{{ post.title }}</a></td>
    <td>{{ post.date | date: "%a, %b %d, %y" }}</td>
  </tr>
{% endfor %}
</table>
