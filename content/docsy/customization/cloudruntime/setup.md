---
title: "本地搭建"
linkTitle: "本地搭建"
weight: 200
description: >
  本地从头开始搭建cloudruntime的编辑环境。
---

简单记录备忘和快速重复搭建。

## 准备工作

先安装好：

- go

- nodejs

- hugo

  `vi ~/.zshrc` 增加一个 h alias，方便后续使用:

  ```bash
  alias h='hugo -D -F server --disableFastRender --bind "0.0.0.0"'
  alias h2='hugo -D -F server --disableFastRender --bind "0.0.0.0" --port 2323'
  alias h3='hugo -D -F server --disableFastRender --bind "0.0.0.0" --port 3333'
  alias h4='hugo -D -F server --disableFastRender --bind "0.0.0.0" --port 4343'
  ```

- git

- markdown编辑器：如 typora

准备好目录，

```bash
mkdir -p ~/work/code/cloudruntime
cd ~/work/code/cloudruntime
```

## 主题仓库

docsy 仓库：

```bash
cd ~/work/code/cloudruntime
git clone git@github.com:cloudruntime/docsy.git 

cd docsy
git checkout cloudruntime

npm install --save-dev autoprefixer
npm install --save-dev postcss-cli
npm install -D postcss
```

## 内容仓库

### website 仓库

```bash
cd ~/work/code/cloudruntime
git clone git@github.com:cloudruntime/website.git

cd website
# start hugo locally
# h
```

### docs 仓库

```bash
cd ~/work/code/cloudruntime
git clone git@github.com:cloudruntime/docs.git

cd docs
# start hugo locally
# h
```

### devdocs 仓库

```bash
cd ~/work/code/cloudruntime
git clone git@github.com:cloudruntime/devdocs.git

cd devdocs
# start hugo locally
# h
```