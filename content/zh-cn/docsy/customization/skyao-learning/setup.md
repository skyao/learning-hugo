---
title: "本地搭建"
linkTitle: "本地搭建"
weight: 70
description: >
  本地从头开始搭建学习笔记的编辑环境。
---

简单记录备忘和快速重复搭建。

## 准备工作

先安装好：

- go

- nodejs

- hugo

  `vi ~/.zshrc` 增加一个 h alias，方便后续使用:

  ```bash
  # hugo
  alias h='hugo -D -F server --disableFastRender --bind "0.0.0.0"'
  alias h2='hugo -D -F server --disableFastRender --bind "0.0.0.0" --port 2323'
  alias h3='hugo -D -F server --disableFastRender --bind "0.0.0.0" --port 3333'
  alias h4='hugo -D -F server --disableFastRender --bind "0.0.0.0" --port 4343'
  ```

- git

- markdown编辑器：如 typora

准备好目录:

```bash
mkdir -p ~/work/code/learning
cd ~/work/code/learning
```

## 主题和构建脚本

docsy 仓库：

```bash
cd ~/work/code/learning
git clone git@github.com:skyao/docsy-learning.git docsy

cd docsy
git checkout learning2024

npm install --save-dev autoprefixer
npm install --save-dev postcss-cli
npm install -D postcss
```

备注：在腾讯云的轻量服务器上，会遇到莫名其妙的问题，需要把上面三个命令修改为：

```bash
npm install -g postcss postcss-cli autoprefixer
```

以全局安装的方式来规避，具体看附录说明。

docsy-example 仓库

```bash
cd ~/work/code/learning
git clone git@github.com:skyao/docsy-example-learning.git docsy-example

cd docsy-example
git checkout build
cp *.sh ../
```

## 验证

以 hugo 学习笔记为例：

```bash
cd ~/work/code/learning
sh clone.sh learning-hugo

cd learning-hugo/
h

# 或者 
sh server.sh learning-hugo
```

## 附录：腾讯云问题

我在将 skyao.io 域名修改为 skyao.net 域名之后，网站服务器就从香港搬到了国内，继续使用腾讯云的轻量服务器。

然后就遇到一个莫名其妙的问题，之前从来没有遇到过。具体为，在上面的安装过程中，docsy 仓库和docsy-example 仓库安装完成后，在 learning-xxx 的学习笔记仓库下，执行 hugo 命令时会莫名其妙的失败，报错如下：

```bash
Start to build learning-hugo ...
Start building sites …
hugo v0.121.1-00b46fed8e47f7bb0a85d7cfc2d9f1356379b740+extended linux/amd64 BuildDate=2023-12-08T08:47:45Z VendorInfo=gohugoio

Total in 2913 ms
Error: error building site: POSTCSS: failed to transform "scss/main.css" (text/css). Check your PostCSS installation; install with "npm install postcss-cli". See https://gohugo.io/hugo-pipes/postcss/: binary with name "npx" not found
Fail to build html content by hugo, exit
```

当我记得之前都是可以自动下载成功的，而且速度很快，这个方式我在各种服务器上都试过，包括本地机器（windows，linux，macos都有试过），从来没有出现过这个问题。

考虑到这是因为 npm 需要为本项目安装一些依赖，比如前面提到的 postcss postcss-cli autoprefixer，因此尝试增加 --loglevel=silly 参数来查看详细信息：

```bash
$ npm install --save-dev autoprefixer --loglevel=silly
......
npm http fetch GET 200 https://registry.npmjs.org/mkdirp/-/mkdirp-3.0.1.tgz 4559ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/hugo-extended/-/hugo-extended-0.120.4.tgz 4603ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/postcss/-/postcss-8.5.6.tgz 4645ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/autoprefixer/-/autoprefixer-10.4.21.tgz 4688ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/isarray/-/isarray-2.0.5.tgz 4701ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/safe-buffer/-/safe-buffer-5.1.2.tgz 4703ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/safe-buffer/-/safe-buffer-5.1.2.tgz 4703ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/lru-cache/-/lru-cache-6.0.0.tgz 4730ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/caniuse-lite/-/caniuse-lite-1.0.30001747.tgz 4788ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/picomatch/-/picomatch-4.0.3.tgz 4781ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/fdir/-/fdir-6.5.0.tgz 4817ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/log-symbols/-/log-symbols-5.1.0.tgz 4863ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/debug/-/debug-4.4.3.tgz 4874ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/mimic-response/-/mimic-response-3.1.0.tgz 4889ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/get-stream/-/get-stream-6.0.1.tgz 4893ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/commander/-/commander-2.20.3.tgz 4898ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/file-type/-/file-type-3.9.0.tgz 4914ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/pify/-/pify-3.0.0.tgz 4968ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/file-type/-/file-type-6.2.0.tgz 4999ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/entities/-/entities-6.0.1.tgz 5014ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/entities/-/entities-6.0.1.tgz 5054ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/get-stream/-/get-stream-6.0.1.tgz 5052ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/@popperjs/core/-/core-2.11.8.tgz 5130ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/prettier/-/prettier-3.6.2.tgz 5170ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/type-fest/-/type-fest-1.4.0.tgz 5203ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/bootstrap/-/bootstrap-5.2.3.tgz 5330ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/is-stream/-/is-stream-3.0.0.tgz 5549ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/@fortawesome/fontawesome-free/-/fontawesome-free-6.4.2.tgz 6036ms (cache miss)
npm http fetch GET 200 https://registry.npmjs.org/unbzip2-stream/-/unbzip2-stream-1.4.3.tgz 6404ms (cache miss)
npm info run @fortawesome/fontawesome-free@6.4.2 postinstall node_modules/@fortawesome/fontawesome-free node attribution.js
npm info run @fortawesome/fontawesome-free@6.4.2 postinstall { code: 0, signal: null }
npm info run hugo-extended@0.120.4 postinstall node_modules/hugo-extended node postinstall.js
```

发现是卡死在这个 hugo-extended 的 postinstall 阶段。

google 之后发现：

https://discourse.gohugo.io/t/hugo-115-1-and-postcss-do-not-work-well-enough-to-be-usable/45342/9

最后以全局安装 postcss postcss-cli autoprefixer 这三个依赖的方式来规避这个问题，这样就不必每个项目都安装这些依赖了。