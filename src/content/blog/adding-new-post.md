---
author: Ilya Bagrov
pubDatetime: 2022-09-23T15:22:00Z
modDatetime: 2023-12-21T09:12:47.400Z
title: Добавление новых записей
slug: adding-new-posts
featured: true
draft: false
tags:
  - docs
description:
  Некоторые правила и рекомендации для создания или добавления новых записей с использованием темы AstroPaper.
---

Вот некоторые правила/рекомендации, советы и хитрости для создания новых записей в блоге с использованием темы AstroPaper.

## Table of contents

## Frontmatter

  
Frontmatter - это основное место для хранения важной информации о записи в блоге (статье). Frontmatter находится вверху статьи и написан в формате YAML. Узнайте больше о frontmatter и его использовании в [astro documentation](https://docs.astro.build/en/guides/markdown-content/).

Вот список свойств frontmatter для каждой записи:

| Свойство           | Описание                                                                                              | Примечание                                    |
| ------------------ | ----------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| **_title_**        | Заголовок поста (h1)                                                                                  | required<sup>\*</sup>                         |
| **_description_**  | Описание поста. Используется в выдержке из поста и описании сайта поста.                              | required<sup>\*</sup>                         |
| **_pubDatetime_**  | Дата и время публикации в формате ISO 8601.                                                           | required<sup>\*</sup>                         |
| **_modDatetime_**  | Дата и время изменения в формате ISO 8601. (добавляется только в случае изменения блог-поста)         | optional                                      |
| **_author_**       | Автор поста.                                                                                          | default = SITE.author                         |
| **_slug_**         | Пермалинк для поста. Это поле является необязательным, но не может быть пустой строкой. (slug: "")    | default = slugified file name                 |
| **_featured_**     | Отображать ли этот пост в разделе "Избранные" на главной странице.                                    | default = false                               |
| **_draft_**        | Отметить этот пост как "непубликованный".                                                             | default = false                               |
| **_tags_**         | Связанные ключевые слова для этого поста. Записаны в формате массива YAML.                            | default = others                              |
| **_ogImage_**      | OG изображение поста. Полезно для обмена в социальных сетях и SEO.                                    | default = SITE.ogImage or generated OG image  |
| **_canonicalURL_** | Канонический URL (абсолютный), в случае, если статья уже существует на другом источнике.              | default = `Astro.site` + `Astro.url.pathname` |

> Подсказка! Вы можете получить дату и время в формате ISO 8601, запустив `new Date().toISOString()` в консоли. Убедитесь, что удалили кавычки!

Только поля `title`, `description` и `pubDatetime` должны быть указаны в frontmatter.

Заголовок и описание (выдержка) важны для оптимизации для поисковых систем (SEO), и поэтому AstroPaper рекомендует включать их в блог-посты.

`slug` - уникальный идентификатор URL-адреса. Поэтому `slug` должен быть уникальным и отличаться от других постов. Пробел в `slug` должен быть разделен с помощью `-` или `_`, но рекомендуется использовать `-`. Slug автоматически генерируется с использованием имени файла блога. Однако вы можете определить свой `slug` как frontmatter в своем блог-посте.

Например, если имя файла блога `adding-new-post.md` и вы не указываете slug в своем frontmatter, Astro автоматически создаст slug для блога, используя имя файла. Таким образом, slug будет `adding-new-post`. Но если вы указываете `slug` в frontmatter, это переопределит slug по умолчанию. Вы можете узнать больше об этом в [документации Astro](https://docs.astro.build/en/guides/content-collections/#defining-custom-slugs).

Если вы опустите `tags` в блог-посте (другими словами, если тег не указан), будет использоваться тег `others` в качестве тега для этого поста. Вы можете установить тег по умолчанию в файле `/src/content/config.ts`.

```ts
// src/content/config.ts
export const blogSchema = z.object({
  // ---
  draft: z.boolean().optional(),
  tags: z.array(z.string()).default(["others"]), // замените слово "другие" на любое другое.
  // ---
});
```

### Sample Frontmatter


Вот пример frontmatter для поста:

```yaml
# src/content/blog/sample-post.md
---
title: Заголовок поста
author: Автор
pubDatetime: 2022-09-21T05:17:19Z
slug: the-title-of-the-post
featured: true
draft: false
tags:
  - some
  - example
  - tags
ogImage: ""
description: Это тестовый пост.
canonicalURL: https://example.org/my-article-was-already-posted-here
---
```

## Adding table of contents

По умолчанию в посте (статье) не включается содержание (TOC). Чтобы включить TOC, вы должны указать это специальным образом.

Напишите `Table of contents` в формате h2 (## в markdown) и разместите его там, где хотите, чтобы оно появилось в посте.

Например, если вы хотите разместить свое содержание прямо под вводным абзацем (как я обычно делаю), вы можете сделать это следующим образом.

```md
---
# some frontmatter
---

Вот несколько рекомендаций, советов и хитростей для создания новых записей в блоге с использованием темы AstroPaper.

## Table of contents

<!-- the rest of the post -->
```

## Headings

Есть одна важная вещь, касающаяся заголовков. В блоге AstroPaper заголовок (title в frontmatter) используется как основной заголовок поста. Поэтому остальные заголовки в посте должны использовать уровни h2 \~ h6.

Это правило не является обязательным, но настоятельно рекомендуется в целях визуального оформления, доступности и SEO.

## Storing Images for Blog Content

Вот два метода для хранения изображений и их отображения внутри файла markdown.

> Примечание! Если требуется стилизовать оптимизированные изображения в markdown, вы должны: [использовать MDX](https://docs.astro.build/en/guides/images/#images-in-mdx-files).

### Inside `src/assets/` directory (recommended)

Вы можете хранить изображения в каталоге `src/assets/`. Эти изображения будут автоматически оптимизированы Astro через [Image Service API](https://docs.astro.build/en/reference/image-service-reference/).

Вы можете использовать относительный путь или алиасный путь (`@assets/`) для обслуживания этих изображений.

Пример: Предположим, вы хотите отобразить `example.jpg`, путь к которому `/src/assets/images/example.jpg`.

```md
![something](@assets/images/example.jpg)

<!-- ИЛИ -->

![something](../../assets/images/example.jpg)

<!-- Использование тега img или компонента Image не будет работать. ❌ -->
<img src="@assets/images/example.jpg" alt="something">
<!-- ^ Это не правильно! ^ -->
```

> Технически, вы можете хранить изображения в любом каталоге внутри `src`. Здесь `src/assets` - просто рекомендация.

### Inside `public` directory

Вы можете хранить изображения в каталоге `public`. Имейте в виду, что изображения, хранящиеся в каталоге `public`, остаются неизменными Astro, что означает, что они не будут оптимизированы, и вам нужно будет самостоятельно обеспечить оптимизацию изображений.

Для этих изображений вы должны использовать абсолютный путь; и эти изображения можно отображать с помощью [аннотации markdown](https://www.markdownguide.org/basic-syntax/#images-1) или [тега HTML img](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img).

Пример: Предположим, что `example.jpg` находится по пути `/public/assets/images/example.jpg`.

```md
![something](/assets/images/example.jpg)

<!-- ИЛИ -->

<img src="/assets/images/example.jpg" alt="something">
```

## Bonus

### Image compression

При вставке изображений в блог-пост (особенно для изображений из каталога `public`) рекомендуется сжимать изображение. Это повлияет на общую производительность веб-сайта.

Мои рекомендации для сайтов по сжатию изображений:

- [TinyPng](https://tinypng.com/)
- [TinyJPG](https://tinyjpg.com/)

### OG Image

По умолчанию будет использовано изображение OG, если пост не указывает изображение OG. Хотя это и не обязательно, изображение OG, связанное с постом, должно быть указано в frontmatter. Рекомендуемый размер для изображения OG составляет **_1200 X 640_** px.

> С версии AstroPaper v1.4.0 изображения OG будут генерироваться автоматически, если не указаны. Проверьте [обновления](https://astro-paper.pages.dev/posts/dynamic-og-image-generation-in-astropaper-blog-posts/).