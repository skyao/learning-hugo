---
title: "macOS安装Hugo"
linkTitle: "macOS安装"
weight: 30
date: 2024-07-18
description: >
  Hugo在macOS操作系统下的安装
---

## 准备工作

### 安装golang

安装hugo之前，先安装好golang，推荐安装最新版本。

### 安装nodejs/npm

为了使用Google Docsy主题，需要安装nodejs/npm。

在以下网站下载nodejs的安装包：

https://nodejs.org/en/download/package-manager

TODO

## 安装Hugo

在[Hugo Releases](https://github.com/spf13/hugo/releases)页面下载对应操作系统版本的安装包。

> 注意：由于Google Docsy主题的需求，需要 extended 版本的hugo 以支持 SCSS:

> 备注: mac 下安装最简单的方式是用brew命令，但不合适用来安装extended版本，所以还是需要下载之后手工安装

下载下列文件：

```bash
wget https://github.com/gohugoio/hugo/releases/download/v0.121.1/hugo_extended_0.121.1_darwin-universal.tar.gz
```

解压后，将 hugo 可执行文件放在path路径下即可：

```
cd hugo_extended_0.121.1_darwin-universal
sudo cp hugo /usr/local/bin/hugo
```



