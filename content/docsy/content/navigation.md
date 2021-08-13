---
title: "导航"
date: 2021-08-13
weight: 230
icon: fas fa-tools
description: >
  docsy的导航
---

> 参考：https://www.docsy.dev/docs/adding-content/navigation/

### 顶级菜单

每个 section 都可以通过 weight 参数指定在顶级菜单中的排序，从小到大，从左到右排序：

```toml
---
title: "Welcome to Docsy"
linkTitle: "Documentation"
menu:
  main:
    weight: 30
    pre: <i class='fas fa-paper-plane'></i>
---
```

注意这里可以通过 pre 设置每个菜单项的css，如为每个菜单项设置一个图标，非常实用。

图标来自 Font Awesome，具体选择可以参考：

- https://fontawesome.com/v5.15/icons?d=gallery&p=2 内容有点多
- http://www.fontawesome.com.cn/faicons/ ：看这里会比较简单直接

也可以单独增加一个带链接的菜单项，修改 config.toml：

```toml
[[menu.main]]
    name = "GitHub"
    weight = 50
    url = "https://github.com/google/docsy/"
    pre = "<i class='fab fa-github'></i>"
    post = "<span class='alert'>New!</span>" 
```

这里同样可以使用 pre 来增加图标，用 post 来增加说明。

### 左侧导航菜单的设置

默认显示section所有的页面，当页面数量较多/层次较深时不太好看，而且太长也不好找内容。

可以通过 params.ui 参数来控制 sidebar 的显示：

```toml
[params.ui]
# Enable to show the side bar menu in its compact state.
sidebar_menu_compact = true
ul_show = 1
sidebar_menu_foldable = true
sidebar_cache_limit = 100
```

具体解释参见： https://www.docsy.dev/docs/adding-content/navigation/#section-menu-options

### 为每个页面加图标

可以在左侧的 sidebar 中为每个页面设置图标，如本页面展示的：

```toml
title: "导航"
date: 2021-08-13
weight: 230
icon: fas fa-tools
```

### 手工添加链接到sidebar

可以手工添加一个链接到左侧的 sidebar，新建一个页面，内容如下：

```toml
---
title: "手工链接1"
date: 2021-08-13
weight: 231
manualLink: "https://skyao.io"
description: >
  docsy的手工链接展示
---
```

也可以通过 manualLinkRelref 指定一个 hugo 的 relref：

```toml
---
title: "手工链接2"
date: 2021-08-13
weight: 232
manualLinkRelref: "/docsy/content/navigation"
description: >
  docsy的手工链接展示2
---
```

详见：https://www.docsy.dev/docs/adding-content/navigation/#add-manual-links-to-the-section-menu

同样也可以通过 icon 来为链接增加图标。

### 搜索

我使用 google custom search engine，具体见 [启用CSE搜索]({{< relref "/docs/theme/docsy/gcse" >}})