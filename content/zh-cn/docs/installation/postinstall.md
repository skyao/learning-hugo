---
title: "安装后配置Hugo"
linkTitle: "安装后配置"
weight: 40
date: 2024-07-18
description: >
  Hugo安装完成后的配置工作
---

## 设置别名

为了方便使用，增加 hugo server 命令的 alias 用来本地编辑时的实时预览： 

```bash
vi ~/.zshrc
```

增加内容：

```bash
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

## 设置代理

### npm代理

主要是 npm 命令需要代理才能顺利下载文件，比如：

```bash
npm install -D --save autoprefixer
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


### 通用代理

如果之前配置有通用代理命令 proxyon，则执行 proxyon 开启各种代理即可：

```bash
# proxy
alias proxyon='export all_proxy=socks5://192.168.2.1:7891;export http_proxy=http://192.168.2.1:7890;export https_proxy=http://192.168.2.1:7890;export no_proxy=127.0.0.1,localhost,local,.local,.lan,192.168.0.0/16,10.0.0.0/16'
alias proxyoff='unset all_proxy http_proxy https_proxy no_proxy'
```

或者临时性的在当前终端中执行：

```bash
export all_proxy=socks5://192.168.2.1:7891
```

