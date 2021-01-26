---
title: "启用Ugly URL"
date: 2021-01-19
weight: 333
description: >
  启用hugo的ugly URL
---

{{% pageinfo %}}
Pretty URL 造成了图片路径的困扰：本地实时编辑的路径和在浏览器中正确访问的路径无法同时满足。
{{% /pageinfo %}}

## 背景

https://gohugo.io/content-management/urls/#ugly-urls

### Pretty URL

Hugo的默认行为是用 "pretty" URL来呈现内容。 Pretty URL不需要任何非标准的服务器端配置就可以工作。

下面演示一下这个概念：

```
content/posts/_index.md
=> example.com/posts/index.html
content/posts/post-1.md
=> example.com/posts/post-1/
```

### Ugly URL

如果想拥有通常所说的 "Ugly URL" (例如example.com/urls.html)，请在网站的 `config.toml`或`config.yaml`中分别设置 `uglyurls = true` 或 `uglyurls: true`。也可以在运行hugo或hugo服务器时，将 `HUGO_UGLYURLS` 环境变量设置为 `true`。



```
.
└── content
    └── about
    |   └── _index.md  // <- https://example.com/about/
    ├── posts
    |   ├── firstpost.md   // <- https://example.com/posts/firstpost/
    |   ├── happy
    |   |   └── ness.md  // <- https://example.com/posts/happy/ness/
    |   └── secondpost.md  // <- https://example.com/posts/secondpost/
    └── quote
        ├── first.md       // <- https://example.com/quote/first/
        └── second.md      // <- https://example.com/quote/second/
```

开启 `uglyurls`  之后的路径变成这样：

```
.
└── content
    └── about
    |   └── _index.md  // <- https://example.com/about.html
    ├── posts
    |   ├── firstpost.md   // <- https://example.com/posts/firstpost.html
    |   ├── happy
    |   |   └── ness.md    // <- https://example.com/posts/happy/ness.html
    |   └── secondpost.md  // <- https://example.com/posts/secondpost.html
    └── quote
        ├── first.md       // <- https://example.com/quote/first.html
        └── second.md      // <- https://example.com/quote/second.html
```

## Pretty URL的影响

Hugo默认采用 pretty URL，url的确漂亮了，但是带来一个很严重的问题，这个问题的来源是编辑内容时最常用的一个需求：

> 如果我们在页面中要插入一张图片，地址该怎么写？

比如我们在某个 docs 目录下，有 a.md , b.md 两个文件和一个 images 目录，images目录下有一个图片文件 `123.jpg`。如果我们要在 a.md 的内容中插入这个  `123.jpg` 图片，那么最常见的方式是:

```markdown
文本内容1

![](images/123.jpg)

文本内容2
```

在本地用markdown编辑软件输入内容时，上面的写法可以直接在软件中看到插入的图片。但是在使用 hugo 渲染之后，用浏览器打开 a.md 的地址:

- 如果是默认的Pretty URL：则地址为 `https://localhost:1313/docs/a/`，此时插入图片的地址按照相对地址 `images/123.jpg` 进行计算，结果是 `https://localhost:1313/docs/a/images/123.jpg`
- 如果开启的Ugly URL：则地址为 `https://localhost:1313/docs/a.html`，此时插入图片的地址按照相对地址 `images/123.jpg` 进行计算，结果是 `https://localhost:1313/docs/images/123.jpg`

在浏览器中的访问结果就是 Pretty URL 下图片无法显示（404错误），Ugly URL下则地址正确可以正常显示图片。

原因在于 Pretty URL 下 a.md 文件映射的访问地址变成了 `https://localhost:1313/docs/a/`这样一个文件夹，而不是 a.html 这样一个文件，导致相对路径计算之后和实际本地文件存储的路径不一致。

要修复这个问题，只要简单的修改图片的相对路径，增加 `../` :

```
文本内容1

![](../images/123.jpg)

文本内容2
```

这样可以正确的到图片的路径，图片可以正常显示。

但是，在markdown编辑软件中进行本地编辑时，由于这个路径和本地文件和图片的相对路径不符，会造成在编辑器中无法正常显示。

总之，虽然 Pretty URL 下URL的确是 Pretty 了，但是却造成了图片路径的困扰：本地实时编辑的路径和在浏览器中正确访问的路径无法同时满足。

## 启用Ugly URL

在没有找到其他可行方法之前（希望有吧），暂时采用启用 Ugly URL 的方式来回避这个问题。

修改 `config.toml` 文件，加入

```yaml
uglyurls = true
```

考虑到所有的笔记都应该保持一致，因此我直接修改了 docsy-example 中 `config.toml` 文件。

备注：如果有朋友知道更好的解决方式，请不吝赐教。

## 不完美的Ugly URL

启用 Ugly URL之后，普通文件如 `docs/a.md` / `docs/b.md` 都采用 `docs/a.html` / `docs/b.html` 这样的文件名和路径，但是对于 `docs/_index.md` ，hugo 有些奇怪的并没有如我预期的转为 `docs/ index.html`，而是转为 `docs.html`。这是一个非常的令人费解的行为，同样造成了在  `docs/_index.md` 文件中图片路径和本地实际路径会不一致的问题。

https://github.com/gohugoio/hugo/issues/4428