---
title: "安装Hugo"
linkTitle: "安装Hugo"
weight: 210
date: 2024-07-18
description: >
  Hugo在多个操作系统下的安装
---

## 准备工作

### 安装golang

安装hugo之前，先安装好golang，推荐安装最新版本。

### 安装nodejs/npm

为了使用Google Docsy主题，需要安装nodejs/npm。

在以下网站下载nodejs的安装包：

https://nodejs.org/en/download/package-manager

#### Linux下安装nodejs

```bash
# installs nvm (Node Version Manager)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# download and install Node.js (you may need to restart the terminal)
nvm install 20

# verifies the right Node.js version is in the environment
node -v # should print `v20.15.1`

# verifies the right npm version is in the environment
npm -v # should print `10.7.0`
```

## 安装Hugo

在[Hugo Releases](https://github.com/spf13/hugo/releases)页面下载对应操作系统版本的安装包。

注意：由于Google Docsy主题的需求，需要 extended 版本的hugo 以支持 SCSS:

![](images/hugo-extended-version.jpg)

### Linux安装

找到linux的安装包，对于 ubuntu 可以直接用 deb 文件：

- hugo_extended_0.121.1_linux-amd64.deb

```bash
wget https://github.com/gohugoio/hugo/releases/download/v0.121.1/hugo_extended_0.121.1_linux-amd64.deb
```

deb文件直接安装即可。

```bash
sudo dpkg -i hugo_extended_0.121.1_linux-amd64.deb
```

验证安装版本：

```bash
$ hugo version
hugo v0.121.1-00b46fed8e47f7bb0a85d7cfc2d9f1356379b740+extended linux/amd64 BuildDate=2023-12-08T08:47:45Z VendorInfo=gohugoio
```


### Mac安装(有待更新)

mac 下安装最简单的方式是用brew命令，但不合适用来安装extended版本，所以还是需要下载之后手工安装，如下载下列文件：

```bash
hugo_extended_0.80.0_macOS-64bit.tar.gz
```

解压后，将 hugo 可执行文件放在path路径下即可：

```
cd hugo_extended_0.80.0_macOS-64bit
sudo cp hugo /usr/local/bin/hugo
# 如果想多个hugo版本共存，可以将hugo二进制文件改名
sudo cp hugo /usr/local/bin/hugo2
```

### Windows安装

TODO：暂未尝试，待更新。

### 安装后设置

验证安装：

```bash
$ hugo version
hugo v0.121.1-00b46fed8e47f7bb0a85d7cfc2d9f1356379b740+extended linux/amd64 BuildDate=2023-12-08T08:47:45Z VendorInfo=gohugoio
```

为了方便使用，增加 hugo server 命令的 alias 用来本地编辑时的实时预览： 

```bash
vi ~/.zshrc
# hugo
alias h='hugo -D -F server --disableFastRender --bind "0.0.0.0"'
alias h2='hugo -D -F server --disableFastRender --bind "0.0.0.0" --port 2323'
alias h3='hugo -D -F server --disableFastRender --bind "0.0.0.0" --port 3333'
alias h4='hugo -D -F server --disableFastRender --bind "0.0.0.0" --port 4343'
```

hugo命令行参数说明：

- `-D`:  等同`--buildDrafts`，标记为 Draft 的内容也会一起构建，方便在本地编写和预览新的暂时未发布的内容。
- `-F`:  等同`--buildFuture`，发布时间为"未来"(即时间比当前时间还要晚)内容也会一起构建，方便在本地编写和预览新的暂时未发布的内容。
- `--disableFastRender`：当内容修改时，进行完整的重新构建，避免预览的内容不够新

h2/h3/h4 指定了不同的端口，当需要在本地打开多个时，可以使用固定端口而不是随机端口。

## 安装SCSS

SCSS的安装参考 docsy 的文档：

https://www.docsy.dev/docs/getting-started/#install-postcss

首先需要安装 nodejs，这样才能使用 npm 命令，然后在hugo项目下执行：

```bash
% npm install -D --save autoprefixer

+ autoprefixer@9.8.6
added 115 packages from 112 contributors and audited 115 packages in 12.974s

% npm install -D --save postcss-cli

+ postcss-cli@7.1.2
added 1 package from 1 contributor, removed 1 package, updated 18 packages and audited 115 packages in 39.211s
```

如果发生报错，并且查看到如下的错误信息：

```bash
836 error path /home/sky/work/code/learning/docsy/node_modules/hugo-extended
837 error command failed
838 error command sh -c node postinstall.js
839 error ✖ Hugo installation failed. :(
839 error node:internal/process/promises:391
839 error     triggerUncaughtException(err, true /* fromPromise */);
839 error     ^
839 error
839 error RequestError: getaddrinfo ENOTFOUND github.com
```

这说明是网络出了问题，导致无法访问 github.com ，需要设置网络代码：


```bash
npm config set proxy http://192.168.2.1:7890
npm config set https-proxy http://192.168.2.1:7890
```

参考：

- https://stackoverflow.com/questions/21509252/nodejs-module-npm-install-error-gcm-node-module-send-err


## 自动发布（归档备忘）

以下是jenkins自动生成并发布到nginx的简单脚本：

```bash
sh update_academic.sh

# clean 
cd /var/www/skyao/
# 删除所有文件和文件夹，但排除以"learning-"前缀开头的
rm -rf `ls | grep -v "learning-"`
cd /var/lib/jenkins/workspace/skyao.io/

# 需要明确指定baseUrl
hugo --baseUrl="https://skyao.io/" -d "/var/www/skyao/"
```

