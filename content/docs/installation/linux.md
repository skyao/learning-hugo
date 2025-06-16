---
title: "linux安装Hugo"
linkTitle: "linux安装"
weight: 10
date: 2024-07-18
description: >
  Hugo在linux操作系统下的安装
---

## 准备工作

### 安装golang

安装hugo之前，先安装好golang，推荐安装最新版本。

### 安装nodejs/npm

为了使用Google Docsy主题，需要安装nodejs/npm。

在以下网站下载nodejs的安装包：

https://nodejs.org/en/download/package-manager

```bash
# installs nvm (Node Version Manager)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash

# download and install Node.js (you may need to restart the terminal)
nvm install 22

# verifies the right Node.js version is in the environment
node -v # should print `v22.11.0`

# verifies the right npm version is in the environment
npm -v # should print `10.9.0`
```

## 安装Hugo

在[Hugo Releases](https://github.com/spf13/hugo/releases)页面下载对应操作系统版本的安装包。

> 注意：由于Google Docsy主题的需求，需要 extended 版本的hugo 以支持 SCSS.

由于 hugo和主题之间版本有依赖关系，因此我们暂时固定使用 v0.121.1 版本。

找到linux的安装包，对于 ubuntu 可以直接用 deb 文件：

- hugo_extended_0.121.1_linux-amd64.deb

```bash
wget https://github.com/gohugoio/hugo/releases/download/v0.121.1/hugo_extended_0.121.1_linux-amd64.deb
```

deb文件直接安装即可。

```bash
sudo dpkg -i hugo_extended_0.121.1_linux-amd64.deb
```

为了避免 apt 自动升级 hugo 版本造成不可用，可以锁定版本：

```bash
sudo apt-mark hold hugo
```


