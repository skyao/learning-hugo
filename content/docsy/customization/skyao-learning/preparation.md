---
title: "定制化的准备工作"
linkTitle: "准备工作"
weight: 20
description: >
  准备fork仓库，建立分支
---

为了避免遗忘，记录定制化的全过程。

以学习笔记为例，时间为 2023/12/27. 

### 安装hugo

node 安装版本：

- Node.js v20.10.0 to /usr/local/bin/node
- npm v10.2.3 to /usr/local/bin/npm

hugo 安装版本为：

- hugo v0.121.1 extended 版本

### Fork 仓库

docs：

从官方仓库： https://github.com/google/docsy

Fork 为自己的仓库：https://github.com/cloudruntime/docsy-learning

docs-example：

从官方仓库：https://github.com/google/docsy-example

Fork 为自己的仓库：https://github.com/cloudruntime/docsy-example-learning

### 克隆代码

> 注意：
>
> - 一定要确保 fork 后的仓库的 master 分支上是干净的，即不做任何代码修改，只用于从 upsream 仓库拉取最新的代码。
>
> - 所有自己的代码改动，在自己 fork 的仓库中新建新的branch

clone 仓库代码到本地，

```bash
mkdir learning
cd learning
git clone git@github.com:skyao/docsy-learning.git docsy
git clone git@github.com:skyao/docsy-example-learning.git docsy-example
```

### 建立分支

然后建立 local-files 分支并增加 upstream。

docsy：

```bash
cd docsy
git checkout -b local-files
git push --set-upstream origin local-files

# 这里直接重用上一次cloudruntime制作好的 local-files 分支
git remote add upstream git@github.com:cloudruntime/docsy.git

git fetch -a
git merge upstream/local-files
git push
```

这样就搞定了本地文件化的事情。

docsy-example：

```bash
cd docsy-example
git checkout -b learning2024
git push --set-upstream origin learning2024

git remote add upstream git@github.com:google/docsy-example.git
git fetch -all
```

### 运行

docsy 这边要先安装 npm 依赖：

```bash
cd docsy
npm install --save-dev autoprefixer
npm install --save-dev postcss-cli
npm install -D postcss
```

docsy-example 这边要修改一下 对 docsy 的依赖，hugo新版本有内置支持，只要修改 hugo.toml 文件，取消 workspace 的注释即可：

```yaml
[module]
  # Uncomment the next line to build and serve using local docsy clone declared in the named Hugo workspace:
  # workspace = "docsy.work"
  workspace = "docsy.work"
```

然后执行:

```bash
alias h='hugo -D -F server --disableFastRender --bind "0.0.0.0"'
```

为了简单起见，修改 `~/.zshrc` 增加 alias：

```bash
alias h='hugo -D -F server --disableFastRender --bind "0.0.0.0"'
// 以后只要敲一个 h 就可以跑起来了
h
```

这时候就可以打开 http://localhost:1313/ 访问了。

此时的代码完全是上游的代码，除了将依赖关系修改为本地之外，还没有做任何修改。

提交代码改动，此时准备工作完成。
