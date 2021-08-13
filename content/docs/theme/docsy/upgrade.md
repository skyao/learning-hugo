---
title: "更新到最新版本"
date: 2021-08-12
weight: 339
description: >
  更新到 docsy 最新版本
---

{{% pageinfo %}}
由于改动太大，每次升级docsy版本时都很难自动merge，基本都要重做一次
{{% /pageinfo %}}

## 建立更新需要的git分支

### 升级 Docsy 最新版本到 master分支

docsy仓库需要 fork，master branch保持和上游master branch的代码一致，master分支上不做任何改动：

升级最新版本时，通过 git remote 机制从上流拉取最新的代码到这个仓库的master分支

### 创建 learning-xxxx 分支

通过下列命令在 docsy 仓库中为每次更新建立单独的 learning 分支，以月份为后缀：

```bash
git checkout -b learning-202108
```

### 运行 docsy-example


1. 进入 docsy 仓库拉取 docsy 依赖的 submodule ：

   ```bash
   cd docsy
   # git checkout leanring-202108
   git submodule update --init --recursive
   ```

2. 删除 docsy-example 仓库下原有的 theme 目录和 .gitmodule 文件

    ```bash
    cd docsy-example
    # git checkout leanring-202108
    rm -rf themes
    rm .gitmodule
    ```

3. 在 docsy-example 仓库下创建到 docsy 仓库的软链接

    ```bash
    mkdir themes
    cd themes
    ln -s ../../docsy .
    ```

	此时运行 hugo 命令就可以构建完全是最新内容的 docsy-example 网站，后面将在这个基础上进行定制化。
	
4. 在 docsy-example 仓库下提交上面修改的代码
	
  ```bash
  git add -A
  git commit -m "remove gitsubmodule of themes; add soft link to docsy"
  ```

## 更新 docsy 

### 文件本地化


```bash
cd docsy/static
mkdir local
cd local

wget -x https://code.jquery.com/jquery-3.5.1.min.js
wget -x https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js
wget -x https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/js/bootstrap.min.js
wget -x https://cdn.jsdelivr.net/gh/rastikerdar/vazir-font@v27.0.1/dist/font-face.css
wget -x https://fonts.gstatic.com/s/opensans/v23/mem5YaGs126MiZpBA-UN_r8OUuhpKKSTjw.woff2
wget -x https://fonts.gstatic.com/s/opensans/v23/mem8YaGs126MiZpBA-UFVZ0bf8pkAg.woff2

wget -x https://fonts.gstatic.com/s/opensans/v23/mem5YaGs126MiZpBA-UN7rgOUuhpKKSTjw.woff2
wget -x http://localhost:1313/webfonts/fa-solid-900.woff2



wget -x https://cdn.jsdelivr.net/npm/mermaid@8.9.2/dist/mermaid.min.js
wget -x https://cdn.jsdelivr.net/npm/katex@0.13.2/dist/katex.min.css
wget -x https://cdn.jsdelivr.net/npm/katex@0.13.2/dist/katex.min.js
wget -x https://cdn.jsdelivr.net/npm/katex@0.13.2/dist/contrib/mhchem.min.js

wget -x https://cdn.jsdelivr.net/npm/katex@0.13.2/dist/contrib/auto-render.min.js
wget -x https://cdn.jsdelivr.net/gh/rastikerdar/vazir-font@v27.0.1/dist/font-face.css

```

### css文件的优化

先不优化，先搞定中文页面，再看是否需要优化，有些内容可能和多语言有关系。

css文件比较麻烦，主要是这些css文件不是以静态文件的方式发布，而是服务器端下发，如果简单改会出现css不对应的问题。之前遇到的代码高亮无法显示就是这个造成的。

```bash
find . -type f -exec grep -Hn "css?family=" {} \;                             
./assets/scss/_variables.scss:67:$web-font-path: "https://fonts.googleapis.com/css?family=#{$google_font_family}&display=swap";
./assets/vendor/bootstrap/site/content/docs/4.6/examples/blog/index.html:5:  - "https://fonts.googleapis.com/css?family=Playfair+Display:700,900"
```

这两个参数是写死的，理论上可以强行静态化，就是涉及到的文件数量非常多。

```
body:lang(he) {
    @import url('https://fonts.googleapis.com/css2?family=Rubik:wght@300;400;500;600;700&display=swap');
    font-family: 'Rubik', "Open Sans", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
}

body:lang(ar) {
    @import url('https://fonts.googleapis.com/css2?family=Tajawal:wght@300;400;500;700&display=swap');
    font-family: 'Tajawal', "Open Sans", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
}
```

> 暂时先不懂，以后再看是否有必要。

### 更新相关的仓库

之后使用 docsy 的 learning-xxx 分支即可，docsy-example 仓库检查了一下，基本没有实质性的内容改动，可以忽略不计。

由于我使用的是 soft link，所以其他笔记自动就更新到了最新的  learning-xxx 分支的 docsy 主题。

