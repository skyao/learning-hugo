---
title: "本地搭建"
linkTitle: "本地搭建"
weight: 70
description: >
  本地从头开始搭建博客网站的编辑环境。
---

简单记录备忘和快速重复搭建。

## 准备工作

先安装好：

- go

- nodejs

- hugo

  `vi ~/.zshrc` 增加 h alias，方便后续使用:

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
mkdir -p ~/work/code/skyao
cd ~/work/code/skyao
```

## 主题和构建脚本

hugo-blox-builder 仓库：

```bash
cd ~/work/code/skyao
git clone git@github.com:skyao/hugo-blox-builder.git
cd hugo-blox-builder
git checkout skyao-io-2024
```

skyao.net 仓库

```bash
# skyao.net.git
cd ~/work/code/skyao
git clone git@github.com:skyao/skyao.net.git
cd skyao.net
```

## 启动 hugo server


```bash
# run hugo server
h
```

输出为：

```bash
Watching for changes in /home/sky/work/code/skyao/{hugo-blox-builder,skyao.net}
Watching for config changes in /home/sky/work/code/skyao/skyao.net/config/_default, /home/sky/work/code/skyao/hugo-blox-builder/modules/blox-plugin-netlify/config.yaml, /home/sky/work/code/skyao/hugo-blox-builder/modules/blox-plugin-reveal/config.yaml, /home/sky/work/code/skyao/hugo-blox-builder/modules/blox-bootstrap/hugo.yaml, /home/sky/work/code/skyao/skyao.net/go.mod
Start building sites … 
hugo v0.121.1-00b46fed8e47f7bb0a85d7cfc2d9f1356379b740+extended linux/amd64 BuildDate=2023-12-08T08:47:45Z VendorInfo=gohugoio


                   |  ZH   
-------------------+-------
  Pages            |  227  
  Paginator pages  |    7  
  Non-page files   | 1154  
  Static files     |  113  
  Processed images | 3394  
  Aliases          |   48  
  Sitemaps         |    1  
  Cleaned          |    0  

Built in 16862 ms
Environment: "development"
Serving pages from memory
Web Server is available at http://localhost:1313/ (bind address 0.0.0.0) 
Press Ctrl+C to stop
```

我的博客网站内容有点多，启动速度要17秒，有点慢。