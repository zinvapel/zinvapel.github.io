---
layout: page
title: Список постов
permalink: /posts
category: about
date: 2017-10-25 21:08:00+03:00
ishead: true
noindex: false
---

### Список постов
<table style="border-collapse: collapse; border: none;">
{% for post in site.posts %}
  <tr>
    <td style="font-family: monospace;">{{ post.date | date: "%y, %b %d " }}</td>
    <td><a style="border: none !important;" href="{{ post.url | absolute_url }}">{{ post.title }}</a></td>
  </tr>
{% endfor %}
</table>

### Список полезняшек
<table style="border-collapse: collapse; border: none;">
{% include epost.html date="2018-07-10" url="https://github.com/squizlabs/PHP_CodeSniffer" title="Проверка синтаксиса PHP. Code sniffer." %}
{% include epost.html date="2018-09-15" url="https://github.com/phpstan" title="Статический анализатор PHP. Phpstan." %}
{% include epost.html date="2018-10-04" url="http://jmsyst.com/bundles/JMSAopBundle" title="Аспектно-ориентированное программирование для Symfony" %}
{% include epost.html date="2018-10-09" url="https://github.com/zinvapel/Front-End-Checklist" title="Чек лист вашего фронтенда" %}
{% include epost.html date="2018-10-26" url="https://gist.github.com/zinvapel/b4bff01c52f927ab136a3fa9021966f4" title="Про JWT токены" %}
{% include epost.html date="2018-11-12" url="https://www.perforce.com/blog/git-beyond-basics-using-shallow-clones" title="Ускорение получения зависимостей Git. Shallow clone." %}
{% include epost.html date="2018-11-29" url="https://danielmiessler.com/study/tcpdump/" title="Использование tcpdump." %}
{% include epost.html date="2019-01-09" url="https://www.rabbitmq.com/documentation.html" title="RabbitMQ." %}
</table>
