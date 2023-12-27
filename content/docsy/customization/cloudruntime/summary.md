---
title: "总结"
linkTitle: "总结"
weight: 190
description: >
  总结改动的内容
---

定制完成后，总结改动的内容。

## docsy 仓库

分支情况：

- `main`: 完全不改，只同步 上游文件，保持和 `google/docsy` 仓库 `main` 分支内容的一致
- `local-files`: 以`main` 分支为 code base，只修改远程访问地址为本地地址，实现网络加速，没有任何文字内容改动
- `cloudruntime：` 以`local-files` 分支为 code base，修改内容和文字，为 cloudruntime 网站定制

### local-files 分支

修改了 js / css 等文件的地址。

增加了 feedback 的中文 i18n 内容。

这些内容是通用的，可以用在其他地方，比如后面我的学习笔记在更新时就重用了 local-files 分支的代码。

### cloudruntime 分支

Merge 了 local-files 分支的改动。

额外的功能：

- 增加了 baidu 统计的功能：直接修改了 footer ，hard code 了 cloudruntime 网站的 baidu 统计代码

## docsy-example 仓库

分支情况：

- `main`: 完全不改，只同步 上游文件，保持和 `google/docsy-example` 仓库 `main` 分支内容的一致
- `cloudruntime：` 以`main` 分支为 code base，修改内容和文字，为 cloudruntime 网站定制

### cloudruntime 分支

- 修改 `hugo.toml`
- 修改 `content` 目录下的内容：主要工作在这里

就没有别的改动了。上面这些改动是 cloudruntime 特有的，不适合用在其他网站。
