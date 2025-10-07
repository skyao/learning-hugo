---
title: "定制化的准备工作"
linkTitle: "准备工作"
weight: 20
description: >
  准备fork仓库，建立分支
---

为了避免遗忘，记录定制化的全过程。

以学习笔记为例，时间为 2024/01/16. 

### 安装hugo

node 安装版本：

- Node.js v20.10.0 to /usr/local/bin/node
- npm v10.2.3 to /usr/local/bin/npm

hugo 安装版本为：

- hugo v0.121.1 extended 版本

### Fork 仓库

主题仓库:

从官方仓库： https://github.com/HugoBlox/hugo-blox-builder

Fork 为自己的仓库：https://github.com/skyao/hugo-blox-builder

模版：

从官方仓库： https://github.com/HugoBlox/theme-academic-cv

Fork 为自己的仓库：https://github.com/skyao/theme-academic-cv



### 克隆代码

> 注意：
>
> - 一定要确保 fork 后的仓库的 master 分支上是干净的，即不做任何代码修改，只用于从 upsream 仓库拉取最新的代码。
>
> - 所有自己的代码改动，在自己 fork 的仓库中新建新的branch

clone 仓库代码到本地，

```bash
mkdir skyao
cd skyao
git clone git@github.com:skyao/hugo-blox-builder.git
git clone git@github.com:skyao/theme-academic-cv.git
```

### 建立分支

然后建立 skyao-io-2024 分支并增加 upstream。

hugo-blox-builder:

```bash
cd hugo-blox-builder
git checkout main
git checkout -b skyao-io-2024
git push --set-upstream origin skyao-io-2024

git remote add upstream git@github.com:HugoBlox/hugo-blox-builder.git

git fetch -a
```



### 运行

这时候就可以打开 http://localhost:1313/ 访问了。

不过为了使用本地修改之后的主题，还是需要修改一下 go.mod 文件，增加以下内容，将依赖指向本地 hugo-blox-builder 的 fork 仓库：

```json
// replace for develop when forked repo is in local
replace github.com/HugoBlox/hugo-blox-builder/modules/blox-bootstrap/v5 => ../hugo-blox-builder/modules/blox-bootstrap
replace github.com/HugoBlox/hugo-blox-builder/modules/blox-plugin-netlify => ../hugo-blox-builder/modules/blox-plugin-netlify
replace github.com/HugoBlox/hugo-blox-builder/modules/blox-plugin-reveal => ../hugo-blox-builder/modules/blox-plugin-reveal
```

此时的代码完全是上游的代码，除了将依赖关系修改为本地之外，还没有做任何修改。

提交代码改动，此时准备工作完成。
