---
title: "快速开始"
date: 2021-01-18
weight: 220
description: >
  详细介绍通过Hugo快速开始搭建一个新的站点
---

### 新建站点

```bash
hugo new site /path/tp/your/site
```

提示如下：

```
$ hugo new site learning-cilium
Congratulations! Your new Hugo site is created in /home/sky/work/code/learning/learning-cilium.

Just a few more steps and you're ready to go:

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/, or
   create your own with the "hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files
   with "hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".

Visit https://gohugo.io/ for quickstart guide and full documentation.

```

### 下载模板

选择喜欢的模板，一般下载方式如下：

```bash
cd learning-cilium
git clone git@github.com:digitalcraftsman/hugo-material-docs.git themes/hugo-material-docs
```

为了快速开始使用，一般模板都会提供一个exampleSite，通常将该目录下的内容都复制到站点跟目录下：

```bash
cp -av themes/hugo-material-docs/exampleSite/* .
```

### 运行

直接执行server命令就可以启动：

```bash
hugo server
```

通过浏览器访问 http://localhost:1313/ 就可以看内容，由于上面我们采用的是直接复制exampleSite的内容，所以一般此时看到的就是这个模板的example内容。

之后再进行具体的网站设定，模板修改，添加内容等。

### 提交到github

```bash
echo "# learning-cilium" >> README.md
git init
git add -A
git commit -m "first commit"
git remote add origin git@github.com:skyao/learning-cilium.git
git push -u origin master
```