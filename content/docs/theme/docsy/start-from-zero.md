---
title: "重来备忘录"
date: 2022-01-20
weight: 399
description: >
  每次重新安装系统之后都不得不从头来一次，为了方便，记录一下全过程以备参考
---



## 操作流程

从头开始，全部流程如下，严格按照顺序操作：

```bash
# docsy fork
cd ~/work/code/learning
git clone git@github.com:skyao/docsy.git
cd docsy
git checkout learning-202108
git submodule update --init --recursive

# docsy example
cd ~/work/code/learning
git clone git@github.com:skyao/docsy-example.git
cd docsy-example
git checkout learning-clean

# docsy build
cd ~/work/code/learning
git clone git@github.com:skyao/docsy-example.git docsy-example-build
cd docsy-example-build
git checkout build
cp clone.sh init.sh server.sh addremote.sh ../

# try one project
cd ~/work/code/learning
sh clone.sh hugo
cd learning-hugo
npm install -D --save autoprefixer
npm install -D --save postcss-cli

# build
cd ~/work/code/learning
cd learning-hugo
h

```

搞定！m1 下运行正常。

