---
title: "发布学习笔记"
linkTitle: "发布"
weight: 60
description: >
  记录发布现有学习笔记到nginx的过程，备用
---

## 背景

我的学习笔记发布在 `https://skyao.io/learning-xxx` 这样的地址下，其中 https://skyao.io 这个网站是我的个人博客网站，也是 基于 hugo，使用 nginx 发布。

## 准备工作

### 准备目录和仓库

使用 `~/site/learning` 目录存放所有学习笔记相关的内容，将相关的仓库 clone 到这个目录下：

```bash
mkdir -p ~/site/learning

# docsy 相关的仓库
git clone https://github.com/skyao/docsy.git
git clone https://github.com/skyao/docsy-example.git
git clone https://github.com/skyao/docsy-example.git docsy-example-build
```

### 准备 docsy

```bash
cd docsy
git checkout learning2024

npm install --save-dev autoprefixer
npm install --save-dev postcss-cli
npm install -D postcss
```

### 安装最新的 hugo 0.121.1 版本

```bash
wget https://github.com/gohugoio/hugo/releases/download/v0.121.1/hugo_extended_0.121.1_linux-amd64.tar.gz
tar xvf hugo_extended_0.121.1_linux-amd64.tar.gz
sudo mv hugo /usr/local/bin/hugo121
```

## 发布过程

### clone.sh

### addremote.sh

### publish.sh

### 遇到的问题

中间遇到一个奇怪的错误（其他平台没有遇到）：

```bash
$ hugo

Start building sites … 
hugo v0.121.1-00b46fed8e47f7bb0a85d7cfc2d9f1356379b740+extended linux/amd64 BuildDate=2023-12-08T08:47:45Z VendorInfo=gohugoio

Total in 7829 ms
Error: error building site: POSTCSS: failed to transform "scss/main.css" (text/css). Check your PostCSS installation; install with "npm install postcss-cli". See https://gohugo.io/hugo-pipes/postcss/: binary with name "npx" not found
```

后来在 learning-hugo 目录下，执行 

```bash
cd learing-hugo
npm install postcss-cli
hugo
```

就可以了。这个问题稍后再看是怎么回事。

## 附录

其他学习笔记：

```bash
# 学习笔记的仓库
git clone https://github.com/skyao/learning-servicemesh.git
git clone https://github.com/skyao/learning-vscode.git
git clone https://github.com/skyao/learning-openwrt.git
git clone https://github.com/skyao/learning-microservice.git
git clone https://github.com/skyao/learning-mtls.git
git clone https://github.com/skyao/learning-macos.git
git clone https://github.com/skyao/learning-kubernetes.git
git clone https://github.com/skyao/learning-grpc.git
git clone https://github.com/skyao/learning-git.git
git clone https://github.com/skyao/learning-envoy.git
git clone https://github.com/skyao/learning-docker.git
git clone https://github.com/skyao/learning-xds.git
git clone https://github.com/skyao/learning-consul.git
git clone https://github.com/skyao/learning-azure.git
git clone https://github.com/skyao/learning-actor.git
git clone https://github.com/skyao/learning-mockito.git
git clone https://github.com/skyao/learning-java-aot.git
git clone https://github.com/skyao/learning-jdk.git
git clone https://github.com/skyao/learning-linux-mint.git
git clone https://github.com/skyao/learning-ubuntu-server.git
git clone https://github.com/skyao/learning-computer-hardware.git
git clone https://github.com/skyao/learning-dapr.git
git clone https://github.com/skyao/learning-temporal.git
git clone https://github.com/skyao/learning-spring-cloud.git
git clone https://github.com/skyao/learning-hugo.git
```

以下是还没有更新到 2024 版 docsy 主题的其他学习笔记：

```bash
git clone https://github.com/skyao/learning-sentinel.git
git clone https://github.com/skyao/learning-distributed-transaction.git
git clone https://github.com/skyao/learning-pve.git
git clone https://github.com/skyao/learning-esxi.git
git clone https://github.com/skyao/learning-kafka.git
git clone https://github.com/skyao/learning-flatbuffers.git
git clone https://github.com/skyao/learning-github-action.git
git clone https://github.com/skyao/learning-fortio.git
git clone https://github.com/skyao/learning-webassembly.git
git clone https://github.com/skyao/learning-tokio.git
git clone https://github.com/skyao/learning-inoreader.git
git clone https://github.com/skyao/learning-windows-server.git
git clone https://github.com/skyao/learning-go.git
git clone https://github.com/skyao/learning-drawing-tool.git
git clone https://github.com/skyao/learning-unit.git
git clone https://github.com/skyao/learning-erpc.git
git clone https://github.com/skyao/learning-cloudstate.git
git clone https://github.com/skyao/learning-cloudnative.git
git clone https://github.com/skyao/learning-serverless.git
git clone https://github.com/skyao/learning-reactor.git
git clone https://github.com/skyao/learning-distributed-capability.git
git clone https://github.com/skyao/learning-nginx.git
git clone https://github.com/skyao/learning-prometheus.git
git clone https://github.com/skyao/learning-maven.git
git clone https://github.com/skyao/learning-dns.git
git clone https://github.com/skyao/learning-hessian.git
git clone https://github.com/skyao/learning-cillium.git
git clone https://github.com/skyao/learning-udpa.git
git clone https://github.com/skyao/learning-eventbrige.git
git clone https://github.com/skyao/learning-knative.git
git clone https://github.com/skyao/learning-linkerd2-proxy.git
git clone https://github.com/skyao/learning-linkerd2.git
git clone https://github.com/skyao/learning-istio.git
git clone https://github.com/skyao/learning-proto3.git
git clone https://github.com/skyao/learning-http2.git
```