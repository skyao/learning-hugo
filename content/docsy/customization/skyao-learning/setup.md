---
title: "本地搭建"
linkTitle: "本地搭建"
weight: 70
description: >
  本地从头开始搭建学习笔记的编辑环境。
---

简单记录备忘和快速重复搭建。

## 准备工作

先安装好：

- go

- nodejs

- hugo

  `vi ~/.zshrc` 增加一个 h alias，方便后续使用:

  ```bash
  # hugo
  alias h='hugo -D -F server --disableFastRender --bind "0.0.0.0"'
  alias h2='hugo -D -F server --disableFastRender --bind "0.0.0.0" --port 2323'
  alias h3='hugo -D -F server --disableFastRender --bind "0.0.0.0" --port 3333'
  alias h4='hugo -D -F server --disableFastRender --bind "0.0.0.0" --port 4343'
  ```

- git

- markdown编辑器：如 typora

准备好目录:

```bash
mkdir -p ~/work/code/learning
cd ~/work/code/learning
```

## 主题和构建脚本

docsy 仓库：

```bash
cd ~/work/code/learning
git clone git@github.com:skyao/docsy-learning.git docsy

cd docsy
git checkout learning2024

npm install --save-dev autoprefixer
npm install --save-dev postcss-cli
npm install -D postcss
```

docsy-example 仓库

```bash
cd ~/work/code/learning
git clone git@github.com:skyao/docsy-example-learning.git docsy-example

cd docsy-example
git checkout build
cp *.sh ../
```

## 验证

以 hugo 学习笔记为例：

```bash
cd ~/work/code/learning
sh clone.sh learning-hugo

cd learning-hugo/
h

# 或者 
sh server.sh learning-hugo
```

