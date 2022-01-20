---
title: "重来备忘录"
date: 2022-01-20
weight: 322
description: >
  每次重新安装系统之后都不得不从头来一次，为了方便，记录一下全过程以备参考:
---


```bash
mkdir -p ~/work/code/skyao

# hugo-academic.git
cd ~/work/code/skyao
git clone git@github.com:skyao/hugo-academic.git
cd hugo-academic
git checkout skyao.io

# skyao.io.git
cd ~/work/code/skyao
git clone git@github.com:skyao/skyao.io.git
cd skyao.io

npm install -D --save autoprefixer
npm install -D --save postcss-cli

# run hugo server
h
```

> 备注：偶尔遇到git clone skyao.io 仓库时有文件意外损坏，导致build失败的情况，删除目录重新clone即可。
