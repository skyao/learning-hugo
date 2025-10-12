---
title: "添加内容"
date: 2021-08-13
weight: 210
description: >
  在docsy中添加内容
---

> 参考： [Adding Content | Docsy](https://www.docsy.dev/docs/adding-content/content/)



## content section

Docsy 主题带有三个模版，以提供三种常见的内容章节：

- docs
- blog
- community

> TODO： 看代码还有一个 swagger 不知道要怎么用

### 自定义section

如复制了 example 网站，不想使用所提供的内容部分，只需删除相应的内容子目录。

同样地，如果想添加一个顶层section，只需添加一个新的子目录，不过如果您想使用任何现有的Docsy模板，而不是默认的模板，您需要在每个页面的前言中明确指定布局或内容类型。例如，如果您创建了一个新的目录 `content/en/amazing`，并希望该自定义部分中的一个或多个页面使用 Docsy 的 `docs` 模板，您可以在每个页面的前言中添加 `type: docs`。

```yaml
---
title: "My amazing new section"
weight: 1
type: docs
description: >
    A special section with a docs layout.
---
```

或者，在你的项目的布局目录中，根据现有的模板之一，为你的新栏目创建你自己的页面模板。

> TODO: 理论上只要找到合适的页面模版，就可以在 docsy 主题中支持更多的section。有需要时再研究。

从Hugo 0.76开始，不需要在每一个页面上都指定 `type: blog`，可以在索引页中指定:

```yaml
---
title: "Latest News"
linkTitle: "News"
menu:
  main:
    weight: 30

cascade:
- type: "blog"
---
```

这就方便太多了。不然每个页面都要指定很麻烦的。



### RSS feeds

```toml
rss_sections = ["docs", "docsy"]
```