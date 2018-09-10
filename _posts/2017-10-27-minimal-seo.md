---
layout: post
title: "Минимальный SEO-набор"
date: "2017-10-27"
category:
  - it
  - seo
---
[note-minimal-seo]: {{ "/it/seo/2017/10/26/minimal-seo/" | relative_url }}

{% include image.html src="/assets/it/seo/relevancy.png" %}

Я запустил блог. Но так как он строится на статической платформе, то поиск я решил прикрутить от Яндекса.

Прикрутить его достаточно легко, но вот заставить Яндекс индексировать страницы и находить результаты - задача необычная для backend-разработчика. Отсюда и родилась тема для первого поста.

<!--more-->

Вооружившись руководством [Яндекса](https://yandex.ru/support/webmaster/robot-workings/helping-robot.html) я написал [конспект][note-minimal-seo], на основании которого буду настраивать этот блог. Что нужно сделать:
  - Создать карту сайта
  - Создать `robots.txt`
  - Добавить мета-теги

## Создаем карту сайта

Мой блог базируется на базе GitHub Pages и Jekyll, так что имеет поддержку автоматического создания карты сайта с помощью плагина `jekyll-sitemap`.
Попробуем его.
  - Добавляем строку `gem 'jekyll-sitemap'` в `Gemfile`.
  - Запускаем `bundle`.
  - Добавляем в `_config.yml`.
Получаем

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>http://localhost:4000/it/</loc>
  </url>
  <url>
    <loc>http://localhost:4000/prog/</loc>
  </url>
  <url>
    <loc>http://localhost:4000/seo/</loc>
  </url>
  <url>
    <loc>http://localhost:4000/notes/it/seo/minimal-seo</loc>
    <lastmod>2017-10-27T00:00:00+03:00</lastmod>
  </url>
  <url>
    <loc>http://localhost:4000/posts/it/seo/minimal-seo</loc>
    <lastmod>2017-10-27T00:00:00+03:00</lastmod>
  </url>
  <url>
    <loc>http://localhost:4000/404</loc>
  </url>
  <url>
    <loc>http://localhost:4000/about/</loc>
  </url>
  <url>
    <loc>http://localhost:4000/</loc>
  </url>
  <url>
    <loc>http://localhost:4000/search/</loc>
  </url>
</urlset>
```

Плагин позволяет законфигурировать сборку карты, но он не позволяет задать приоритет и частоту обновлений. Поэтому я воспользуюсь альтернативным способом собрать карту сайта - [средствами Jekyll и Liquid](https://github.com/zinvapel/zinvapel.github.io/blob/master/sitemap.xml). Получаю вот такую карту:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>http://localhost:4000/posts/it/seo/minimal-seo</loc>
    <lastmod>2017-10-27T00:00:00+03:00</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.1</priority>
  </url>
  <url>
    <loc>http://localhost:4000/c-jekyll</loc>
    <lastmod>2017-10-24T00:00:00+03:00</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.1</priority>
  </url>
  <url>
    <loc>http://localhost:4000/notes/it/seo/minimal-seo</loc>
    <lastmod>2017-10-27T00:00:00+03:00</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.1</priority>
  </url>
  <url>
    <loc>http://localhost:4000/it</loc>
    <!-- <lastmod>Отсутствует</lastmod> -->
    <changefreq>always</changefreq>
    <priority>0.5</priority>
  </url>
  <url>
    <loc>http://localhost:4000/seo</loc>
    <!-- <lastmod>Отсутствует</lastmod> -->
    <changefreq>always</changefreq>
    <priority>0.5</priority>
  </url>
  <url>
    <loc>http://localhost:4000/prog</loc>
    <!-- <lastmod>Отсутствует</lastmod> -->
    <changefreq>always</changefreq>
    <priority>0.5</priority>
  </url>
  <url>
     <loc>http://localhost:4000/</loc>
     <!-- <lastmod>Отсутствует</lastmod> -->
     <changefreq>always</changefreq>
     <priority>1</priority>
  </url>
  <url>
     <loc>http://localhost:4000/about</loc>
     <!-- <lastmod>Отсутствует</lastmod> -->
     <changefreq>never</changefreq>
     <priority>0.1</priority>
  </url>
</urlset>
```

## Создадим robots.txt
Первым делом я указываю карту сайта, как известно, она может находится в любом месте сайта.

```text
Sitemap: https://zinvapel.github.io/sitemap.xml
```

Далее я задаю директивы по-умолчанию, то есть для всех ботов:
```text
User-agent: *
Disallow: /search
Disallow: /script
```

Директивы для Яндекса не будут отличаться от остальных на данный момент, поэтому я сохраняю файл таким.

## Добавление мета-тегов
Пожалуй, это самый простой из пунктов. Согласно своего конспекта завел мета-теги `Keywords`, `Description`, `viewport`, `Content-Type`, `<link rel="canonical" ...`. Для роботов указал `<meta name="robots" content="all"/>`.

Здесь я обнаружил удобство хранить некоторые заметки во внешних источниках. Для них я добавил свои значения мета-тегов.

## Итого
Помимо этих настроек, я добавил метрику от Яндекса, чтобы видеть статистику сайта. В общем добавление минимальной SEO-информации не является сложной задачей, любой человек запросто с ней справится.
