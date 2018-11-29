---
layout: page
title: Логи интересных мелочей
permalink: /logs
category: about
date: '2018-10-09'
ishead: false
noindex: false
---

<table style="border-collapse: collapse; border: none;">
  <tr>
    <td style="font-family: monospace;">{{ '2018-10-09' | date: "%y, %b %d " }}</td>
    <td>\[Git] Можно выполнить `merge` без коммита с опцией `--no-commit`</td>
  </tr>
  <tr>
    <td style="font-family: monospace;">{{ '2018-10-09' | date: "%y, %b %d " }}</td>
    <td>\[Git] Чтобы пропустить хуки нужно выполнить `commit` с опцией `--no-verify`</td>
  </tr>
  <tr>
    <td style="font-family: monospace;">{{ '2018-10-10' | date: "%y, %b %d " }}</td>
    <td>\[Git] Чтобы подмержить предыдущую ветку можно воспользоваться командой `git merge -`</td>
  </tr>
  <tr>
    <td style="font-family: monospace;">{{ '2018-11-12' | date: "%y, %b %d " }}</td>
    <td>
        \[Docker] Docker For Mac завис в статусу `Docker is starting`.
        - Подключаемся к терминалу
        - `screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty`
        - Нажимаем пару раз enter. Если ничего не происходит - порестартьте докер. (отключиться от терминала -  Ctrl+A+D ). Ничего не видим, но жмакаем пару раз Enter вслепую, должны побежать буквы и строки загрузки linux kernel. Отключаемся от терминала - Profit!

        Что произошло? При старте виртуальная машина решила что-то спросить, какие-то настройки выставить. Ей никто не отвечает, вот она и ждет. Мы зашли - и что-то повыбирали, она пошла запускаться.
    </td>
    <td>
      \[HTTPS] Чтобы добавить поддержку HTTPS в локальный nginx-сервер, необходимо создать самоподписанный сертификат по инструкции https://ksearch.wordpress.com/2017/08/22/generate-and-import-a-self-signed-ssl-certificate-on-mac-osx-sierra/, после чего добавить полученные файлы в настройки nginx.
      {%- highlight console -%}
listen              443 ssl;
ssl_certificate     /etc/ssl/certs/admin.courier-schedule.yandex-team.ru.crt;
ssl_certificate_key /etc/ssl/private/admin.courier-schedule.yandex-team.ru.key;
      {%- endhighlight -%}
    </td>
  </tr>
</table>
