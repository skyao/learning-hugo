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
- `learning2024` 以 `local-files` 分支为 code base，修改内容和文字，为学习笔记网站定制

### local-files 分支

重用 cloudruntime 的 `local-files` 分支。

### learning2024 分支

Merge 了 local-files 分支的改动。

额外的功能：

- 增加了 baidu 统计的功能：直接修改了 footer ，hard code 了 cloudruntime 网站的 baidu 统计代码

## docsy-example 仓库

分支情况：

- `main`: 完全不改，只同步 上游文件，保持和 `google/docsy-example` 仓库 `main` 分支内容的一致
- `learning2024`: 以`main` 分支为 code base，修改内容和文字，为学习笔记定制
- `learning-clean-2024`: 内容来自 `learning2024`，但清除了git的历史记录，在空白分支上复制 `learning2024` 的内容制作的全新的分支，用于给各个学习笔记同步最新改动用。

### learning2024 分支

- 修改 `hugo.toml`
- 修改 `content` 目录下的内容：主要工作在这里

就没有别的改动了。上面这些改动是学习笔记特有的，不适合用在其他网站。

### learning-clean-2024 分支

复制 `learning2024` 的内容，重新打造。