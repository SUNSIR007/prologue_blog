# Ryuichi Blog

Next.js13 + tailwindcss + contentlayer + MDX Blog

这是一个由Next.js生态拼凑而生的博客（笔者没有任何技术）。

原作者：https://github.com/hxlog/prologue.dev

## Features

- Content Focused, contentlayer + md/mdx
- Adaptive dark mode
- Full SEO, Opengraph + JSON-LD
- Lightweight search engine, powered by Fuse.js

- 专注于内容创作，支持markdown/mdx
- 自适应黑暗模式
- 完整的SEO支持，Opengraph、JSON-LD和RSS
- 轻量级的搜索引擎，Fuse.js实现全文搜索和模糊搜索


## Get Started

You can easily customize this site, all configurations are in `/data`,static files are in `/public`

你可以很容易自定义网站，所有配置文件都在`/data`目录，静态文件存放在`/public`

## Configuration

Post frontmatter

```yaml
---
title: title
description: description
pubDate: 2022-11-13
(required)

updatedDate: 2023-07-02
featured: true
tags: ["tag1",tag2]
image: /static/photos/06.jpg 
imageDesc: This is a static file
(optional)
---
```

Page frontmatter

```yaml
title: title
description: description
(required)
```
