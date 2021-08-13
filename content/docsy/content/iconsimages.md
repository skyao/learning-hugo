---
title: "图标图片"
date: 2021-08-13
weight: 260
description: >
  docsy的图标图片支持，在项目中添加和定制标识、图标和图像。
---

> 参考：[Logos and Images | Docsy](https://www.docsy.dev/docs/adding-content/iconsimages/)



## Logo

在项目中添加项目标识作为 `assets/icons/logo.svg`。这将覆盖主题中默认的 `Docsy` 标志。

如果当前项目没有设置，则默认使用 docsy 模版下的   `assets/icons/logo.svg` 和  `assets/icons/logo.png`。其中在每个页面的左上角出现的 Logo 必须是 要通过设置 svg 图片来覆盖，png 格式的 Logo 文件暂时还没有看到用的地方。

备注：logo 图片不要求是正方形，长方形的图片也是可以的，而且显示效果也还可以。比如 hugo 学习的 Logo 图片就是长条形的。

## favicon

> 最简单的方法是通过 http://cthedot.de/icongen（它可以让你在一张图片上创建大量的图标尺寸和选项）和/或 https://favicon.io ，并把它们放在你的网站项目的 `static/favicons` 目录中。这将覆盖主题中的默认图标。
>
> 请注意，https://favicon.io 并不像Icongen那样可以创建广泛的尺寸，但可以让你快速地从文本中创建图标：如果你想创建文本图标，你可以使用这个网站来生成它们，然后使用 Icongen 从你生成的 .png 文件中创建更多尺寸（如果需要）。
>
> 如果你有特殊的favicon要求，你可以用你的链接创建你自己的 layouts/partials/favicons.html 。

实践：

1. 准备png文件：先找一个项目对应的 Logo 文件，最好是正方形，png格式，背景透明。如果不是，用 photoshop 等工具进行修改，如通过选择工具或者抓图的方式将主体复制出来到新的图片（注意新图片的背景设置为透明），然后将画布大小调整为正方形，四周留一点空白。然后导出为 png 格式。
2. 通过 http://cthedot.de/icongen 网站生成各种格式的 icon 文件，主要在默认的前四个选项的基础上勾选增加第五个 andord app 
3. 将生成的文件下载下来，将各种icon图片复制到当前项目下的  `static/favicons` 目录中

