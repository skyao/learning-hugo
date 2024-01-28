---
title: "安装Hugo"
linkTitle: "安装Hugo"
weight: 210
date: 2021-01-18
description: >
  Hugo在多个操作系统下的安装
---

## 准备工作

### 安装golang

安装hugo之前，先安装好golang，推荐安装最新版本。

### 安装nodejs/npm

为了使用Google Docsy主题，需要安装nodejs/npm。

在以下网站下载nodejs的安装包：

https://nodejs.org/en/download/ 

#### Linux下安装nodejs

```bash
wget https://nodejs.org/dist/v18.12.0/node-v18.12.0-linux-x64.tar.xz
tar xvf node-v18.12.0-linux-x64.tar.xz
sudo mv node-v18.12.0-linux-x64 /usr/share/nodejs
```

打开 `~/.zshrc`, 添加下来内容,将 `/usr/share/nodejs/bin` 加入PATH 路径:

```bash
export PATH=/usr/share/nodejs/bin:$PATH
```

完成之后执行version命令检验是否安装成功：

```bash
source ~/.zshrc
npm --version
```

注意这里一定要将 `/usr/share/nodejs/bin` 加入到 PATH，不然后面安装其他内容后使用时会报错找不到命令，因为通常新安装的命令都会放在 `/usr/share/nodejs/bin`

## 安装Hugo

在[Hugo Releases](https://github.com/spf13/hugo/releases)页面下载对应操作系统版本的安装包。

注意：由于Google Docsy主题的需求，需要 extended 版本的hugo 以支持 SCSS:

![](images/hugo-extended-version.jpg)

### Linux安装

找到linux的安装包，对于 ubuntu 可以直接用 deb 文件：

- hugo_extended_0.105.0_linux-amd64.deb

deb文件直接安装即可。

```bash
sudo dpkg -i hugo_extended_0.105.0_linux-amd64.deb
```

### Mac安装

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
hugo v0.105.0-0e3b42b4a9bdeb4d866210819fc6ddcf51582ffa+extended linux/amd64 BuildDate=2022-10-28T12:29:05Z VendorInfo=gohugoio
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

## 自动发布

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

