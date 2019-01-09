---
layout: page
title: Случайная тема
permalink: /gssl
category: about
date: '2019-01-09'
ishead: true
noindex: false
---

<script>
    let themeList = [
      'nginx',
      'PHP',
      'RabbitMQ',
      'MySQL',
      'PostgreSQL',
      'TCP',
      'ООП',
      'Docker',
      'Python',
      'Frontend',
      'Javascript',
      'Redis',
      'Kafka',
      'Блог программиста',
    ];

    document.addEventListener(
      'DOMContentLoaded',
      function(event) {
          let action = function () {
              let query = document.getElementsByName('q')[0];
              if (!query.value) {
                alert("Введите значение")
                return;
              }
              prev = query.value
              query.value = query.value + ' site:github.io';
              document.gssl.submit()
              query.value = prev
          }
          let rnd = function () {
              let item = themeList[Math.floor(Math.random() * themeList.length)];
              let query = document.getElementsByName('q')[0];
              query.value = item
              action()
          }

          document.getElementById('sbm').addEventListener('click', action)
          document.getElementById('sbm_rnd').addEventListener('click', rnd)
      }
    );
</script>
<form name='gssl' action="https://www.google.com/search" method="GET" target="_blank">
    <input type="text" name="q"/>
    <input type="hidden" name="lr" value="lang_ru">
    <input type="button" id="sbm" value="Перейти">
    <input type="button" id="sbm_rnd" value="Случайное">
</form>
