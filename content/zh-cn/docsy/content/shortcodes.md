---
title: "shortcodes"
date: 2021-08-13
weight: 250
description: >
  docsy的shortcodes支持
---

> 参考：[Docsy Shortcodes | Docsy](https://www.docsy.dev/docs/adding-content/shortcodes/)

Hugo让你定义和使用快捷代码，而不是从头开始编写你的所有网站页面。这些是可重复使用的内容片段，你可以将其包含在你的页面中，通常使用HTML来创造在简单的Markdown中很难或无法做到的效果。短码也可以有参数，例如，让你在一个花哨的短码文本框中添加自己的文字。除了Hugo的内置短代码，Docsy还提供了一些自己的短代码来帮助你建立你的页面。


## Blocks

cover/lead/section/feature/link-down，这是 docsy 首页用到的几个 shortcode。


## helpers

### alert/警告

警报短代码创建了警报块，可用于显示通知或警告：

{{% alert title="Warning" color="primary" %}}
This is a warning.
{{% /alert %}}

color 是 theme colors, 可以是： primary, info, warning 等。

### pageinfo

pageinfo短代码创建了一个文本框，您可以用它来为一个页面添加横幅信息：例如，让用户知道该页面包含占位符内容，该内容已被废弃，或者它记录了一个测试版功能。

{{% pageinfo color="warning" %}}
This is placeholder content.
{{% /pageinfo %}}

### imgproc 

imgproc短码在当前页面包中找到一张图片，并根据一组处理指令对其进行缩放。

> TODO: 没想好什么情况下需要用，平时还是简单的图片插入容易直接看到图片结果

### swaggerui

swaggerui短码可以放置在具有swagger布局的页面内的任何地方；它使用任何OpenAPI YAML或JSON文件作为源文件渲染Swagger UI。这可以托管在你喜欢的任何地方，例如在你的网站的根/static文件夹中。


### iframe

> TODO: 暂时没想到用的地方


## 标签式面板

这个在涉及多种实现，如多个系统/多个编程需要/多个条件时特别有用。

> 有时，在编写内容时，采用标签式窗格是非常有用的。一个常见的用例是显示多个语法高亮的代码块，展示同一个问题，以及如何用不同的编程语言来解决这个问题。作为一个例子，下面的表格显示了著名的Hello world！程序的特定语言变体，当人们从头开始学习一种新的编程语言时，通常会先写这个程序。

Docsy模板提供了两个快捷键tabpane和tab，让你轻松地创建标签式窗格。为了了解如何使用它们，请看下面的代码块，它显示为一个有三个标签的窗格。

{{< tabpane >}}
  {{< tab header="English" >}}
    Welcome!
  {{< /tab >}}
  {{< tab header="German" >}}
    Herzlich willkommen!
  {{< /tab >}}
  {{< tab header="Swahili" >}}
    Karibu sana!
  {{< /tab >}}
{{< /tabpane >}}

标签式面板是通过两个 shortcode 实现的: 

- tabpane 短代码，它是标签的容器元素。这个 shortcode 可以选择性地持有命名的参数lang和/或highlight。这些可选参数的值作为第二个LANG和第三个OPTIONS参数传递给Hugo内置的highlight函数，该函数用于渲染各个标签的代码块。如果标签的标题文本等于标签代码块中使用的语言（如上面第一个标签窗格的例子），你可以在周围的标签窗格短代码中指定langEqualsHeader=true。然后，单个标签的标题文本被自动设置为各自标签的语言参数。

- 各种 tab 短代码，实际上代表了你想显示的标签。我们建议为每个文本指定命名参数header，以便设置每个标签的标题文本。如果需要，你还可以为每个标签指定命名参数lang和highlight。这允许你覆盖在父标签页短代码中给出的设置。如果语言既没有在tabpane中也没有在tabshortcode中指定，它默认为Hugo的站点变量 .Site.Language.Lang 。

## 卡片面板

在编写内容时，有时将类似的文本块或代码片

> TODO: 没用过，看着不错，在需要并排展示，或者对比展示时非常有用。效果会比简单的 list 要好看。


