---
layout: page
title: Список постов
permalink: /posts
category: about
date: 2017-10-25 21:08:00+03:00
ishead: true
noindex: true
---

<table id="post_table" style="border-collapse: collapse; border: none;">
{% for post in site.posts %}
  <tr>
    <td style="font-family: monospace;">{{ post.date | date: "%y, %b %d " }}</td>
    <td><a style="border: none !important;" href="{{ post.url | absolute_url }}">{{ post.title }}</a></td>
  </tr>
{% endfor %}
</table>
