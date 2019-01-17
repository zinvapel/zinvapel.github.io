---
layout: post
title: "[Пост] Публикуем PHP библиотеку в Packagist"
date: '2019-01-17'
category:
  - it
  - tools
---

{% include image.html src="/assets/it/tools/packagist/logo.png" %}
[Composer](https://getcomposer.org/) позволяет использовать кастомные репозитории с помощью блока `repositories`. Чтобы сделать доступным вашу библиотеку всем без указания репозитория, необходимо опубликовать её в [Packagist](https://packagist.org/). Сделать это крайне просто.

<!--more-->
Чтобы перестать копипастить из раза в раз, у меня появилась [библиотека](https://github.com/zinvapel/enumeration) с классом `BaseEnumeration`. При попытке подключить её до размещения на Packagist, будет возникать ошибка:
```
$ composer require zinvapel/enumeration


  [InvalidArgumentException]
  Could not find a matching version of package zinvapel/enumeration. Check the package spelling, your version constraint and that the package is available in a stability which matches your minimum-stab
  ility (stable).


require [--dev] [--prefer-source] [--prefer-dist] [--no-progress] [--no-suggest] [--no-update] [--no-scripts] [--update-no-dev] [--update-with-dependencies] [--update-with-all-dependencies] [--ignore-platform-reqs] [--prefer-stable] [--prefer-lowest] [--sort-packages] [-o|--optimize-autoloader] [-a|--classmap-authoritative] [--apcu-autoloader] [--] [<packages>]...
```

Чтобы сделать библиотеку доступной, необходимо выполнить несколько действий:
- Затегировать его:
```
$ git tag -a 0.1 -m 0.1
$ git push origin master --tags
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Writing objects: 100% (1/1), 162 bytes | 162.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To github.com:zinvapel/enumeration.git
 * [new tag]         0.1 -> 0.1
```

- Регистрируюсь на Packagist [https://packagist.org/register/](https://packagist.org/register/)
- Подключаю Packagist к GitHub [https://packagist.org/profile/edit](https://packagist.org/profile/edit)
- Перехожу во вкладку Submit [https://packagist.org/packages/submit](https://packagist.org/packages/submit)
- Ввожу адрес репозитория
{% include image.html src="/assets/it/tools/packagist/repo.png" %}
- Нажимаю Submit
- Готово!

Теперь во вкладке 'My packages' будет добавлен репозиторий:
{% include image.html src="/assets/it/tools/packagist/repo_view.png" %}

В настройках проекта на GitHub в разделе 'Webhooks' появится новый хук, который ответственнен за публикацию новых версий в Packagist.
{% include image.html src="/assets/it/tools/packagist/hooks.png" %}

Ну и самое главное, библиотека будет доступна всем:
```
$ composer require zinvapel/enumeration 0.1
./composer.json has been updated
Loading composer repositories with package information
Updating dependencies (including require-dev)
Package operations: 1 install, 0 updates, 0 removals
  - Installing zinvapel/enumeration (0.1): Downloading (100%)
Writing lock file
Generating autoload files
```
