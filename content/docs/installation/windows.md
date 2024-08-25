---
title: "windows安装Hugo"
linkTitle: "windows安装"
weight: 20
date: 2024-07-18
description: >
  Hugo在 windows 操作系统下的安装
---

## 准备工作

### 安装golang

安装hugo之前，先安装好golang，推荐安装最新版本。

https://go.dev/dl/

下载 windows 安装文件，如 go1.22.5.windows-amd64.msi。

安装时选择安装目录为 `C:\Users\sky\work\soft\golang`。

修改环境变量，将 GOPATH 的值修改为 `C:\Users\sky\work\soft\gopath`（默认为 %USERPROFILE%\go）。

### 安装nodejs/npm

为了使用Google Docsy主题，需要安装nodejs/npm。

在以下网站下载nodejs的安装包：

https://nodejs.org/en/download/package-manager

选择 prebuild installer，下载到 node-v20.16.0-x64.msi 。

安装时选择安装路径为 `C:\Users\sky\work\soft\nodejs`。

## 安装Hugo

在[Hugo Releases](https://github.com/spf13/hugo/releases)页面下载对应操作系统版本的安装包。

> 注意：由于Google Docsy主题的需求，需要 extended 版本的hugo 以支持 SCSS:

由于 hugo和主题之间版本有依赖关系，因此我们暂时固定使用 v0.121.1 版本。

https://github.com/gohugoio/hugo/releases/download/v0.121.1/hugo_extended_0.121.1_windows-amd64.zip 

下载下来之后，解压缩，将 hugo.exe 文件复制到目录 `C:\Users\sky\work\soft\hugo` 下。

然后修改环境变量，在 Path 中增加一项，路径为 `C:\Users\sky\work\soft\hugo`。

验证安装：

```bash
$ which hugo
/c/Users/sky/work/soft/hugo/hugo

$ hugo version
hugo v0.121.1-00b46fed8e47f7bb0a85d7cfc2d9f1356379b740+extended windows/amd64 BuildDate=2023-12-08T08:47:45Z VendorInfo=gohugoio
```





