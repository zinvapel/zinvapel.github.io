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
    <td style="font-family: monospace;">{{ '2018-10-09' | date: "%d.%m.%Y" }}</td>
    <td>[Git] Можно выполнить <code class="highlighter-rouge">merge</code> без коммита с опцией <code class="highlighter-rouge">--no-commit</code></td>
  </tr>
  <tr>
    <td style="font-family: monospace;">{{ '2018-10-09' | date: "%d.%m.%Y" }}</td>
    <td>[Git] Чтобы пропустить хуки нужно выполнить <code class="highlighter-rouge">commit</code> с опцией <code class="highlighter-rouge">--no-verify</code></td>
  </tr>
  <tr>
    <td style="font-family: monospace;">{{ '2018-10-10' | date: "%d.%m.%Y" }}</td>
    <td>[Git] Чтобы подмержить предыдущую ветку можно воспользоваться командой <code class="highlighter-rouge">git merge -</code></td>
  </tr>
  <tr>
    <td style="font-family: monospace;">{{ '2018-11-12' | date: "%d.%m.%Y" }}</td>
    <td>
        [Docker] Docker For Mac завис в статусу <code class="highlighter-rouge">Docker is starting</code>.
        - Подключаемся к терминалу
        - <code class="highlighter-rouge">screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty</code>
        - Нажимаем пару раз enter. Если ничего не происходит - порестартьте докер. (отключиться от терминала -  Ctrl+A+D ). Ничего не видим, но жмакаем пару раз Enter вслепую, должны побежать буквы и строки загрузки linux kernel. Отключаемся от терминала - Profit!

        Что произошло? При старте виртуальная машина решила что-то спросить, какие-то настройки выставить. Ей никто не отвечает, вот она и ждет. Мы зашли - и что-то повыбирали, она пошла запускаться.
    </td>
  </tr>
  <tr>
    <td style="font-family: monospace;">{{ '2018-11-29' | date: "%d.%m.%Y" }}</td>
    <td>
      [HTTPS] Чтобы добавить поддержку HTTPS в локальный nginx-сервер, необходимо создать самоподписанный сертификат по <a style="border: none; padding: 0;" href ="https://ksearch.wordpress.com/2017/08/22/generate-and-import-a-self-signed-ssl-certificate-on-mac-osx-sierra/">инструкции</a>, после чего добавить полученные файлы в настройки nginx.
<pre>listen              443 ssl;
ssl_certificate     /etc/ssl/certs/sert.crt;
ssl_certificate_key /etc/ssl/private/key.key;</pre>
    </td>
  </tr>
</table>
