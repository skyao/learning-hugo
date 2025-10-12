---
title: "搭建CloudRuntime开发文档"
linkTitle: "CloudRuntime开发文档"
weight: 120
description: >
  修改模版内容，搭建CloudRuntime开发文档
---

准备修改 docsy-example 的内容，搭建CloudRuntime 用户文档。

## 准备仓库

创建 `cloudruntime/devdocs` 仓库，使用 main 分支：

```bash
cd cloudruntime
git clone git@github.com:cloudruntime/devdocs.git
cd devdocs

# 以下方式废弃，会引入原来 docsy 仓库中的所有 commit 内容，造成麻烦
# git remote add upstream git@github.com:cloudruntime/docsy-example.git
# git fetch --all
# git merge upstream/cloudruntime --allow-unrelated-histories

# 直接复制 docs 下的所有内容
cp -r ../docs/* .
# 各种修改
# ......
# 之后一次性提交
git add -A
```



## 修改内容

将 docs 目录重命名为 userguide，增加 example 和 reference 两个类似的子目录。

删除 about, community,blog等内容。

修改各个页面字眼和内容。

## 增加导航条目

类似website，增加一个返回注册的导航条。

修改 hugo.toml，增加如下内容，这样可以在增加导航条的同时实现 i18n：

```toml
[[languages.en.menu.main]]
name = "Home"
weight = 100
url = "https://cloudruntime.net/en/"
pre = "<i class='fa-solid fa-house'></i>"

[[languages.zh-cn.menu.main]]
name = "返回主页"
weight = 100
url = "https://cloudruntime.net/"
pre = "<i class='fa-solid fa-house'></i>"
```

## 发布网站内容

内容发布在 cloudruntime.net/docs 子目录下，因此无需单独建立网站，只要将内容复制到 docs 子目录即可。

在 website/publish.sh 的基础上稍微改动一下发布脚本的细节就好了。
