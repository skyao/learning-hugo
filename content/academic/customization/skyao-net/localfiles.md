---
title: "使用本地文件加速网络访问"
linkTitle: "使用本地文件"
weight: 40
description: >
  使用本地文件加速，避免被墙
---

准备修改 hugo-blox-builder ，将可能会被墙的文件修改为使用本地文件。

为了方便修改，建议用 vs code 之类的 IDE 打开 fork 下来的 hugo-blox-builder 仓库。

## 准备工作

### 准备分支

开一个 local-files 分支，准备修改为本地文件用，后面还是要合并到 skyao-io-2024 分支的：

```bash
cd hugo-blox-builder
git checkout skyao-io-2024
git checkout -b localfiles
git push --set-upstream origin local-files
```

## 准备local目录

在 hugo-blox-builder 仓库下的 blox-bootstrap 模块下建立$ `static/local` 文件夹：

```bash
cd hugo-blox-builder
cd modules/blox-bootstrap/static
mkdir local
cd local
```

## 修改

### 修改 assets.toml 文件

打开 `modules/blox-bootstrap/data/assets.toml` 文件，这个文件中定义了大部分js和css文件的地址和版本。

从地址和版本中获取到下面这些下载地址，将文件下载到 static/local 目录：

```bash
wget -x "https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"
wget -x "https://cdn.jsdelivr.net/gh/metafizzy/isotope@v3.0.6/dist/isotope.pkgd.min.js"
wget -x "https://cdn.jsdelivr.net/npm/imagesloaded@5.0.0/imagesloaded.pkgd.min.js"
wget -x "https://cdn.jsdelivr.net/gh/hpneo/gmaps@0.4.25/gmaps.min.js"
wget -x "https://cdn.jsdelivr.net/npm/leaflet@1.7.1/dist/leaflet.min.js"
wget -x "https://cdn.jsdelivr.net/npm/@fancyapps/fancybox@3.5.7/dist/jquery.fancybox.min.js"
wget -x "https://cdn.jsdelivr.net/gh/krisk/Fuse@v3.2.1/dist/fuse.min.js"
wget -x "https://cdn.jsdelivr.net/gh/julmot/mark.js@8.11.1/dist/jquery.mark.min.js"
wget -x "https://cdn.jsdelivr.net/npm/instantsearch.js@4/dist/instantsearch.production.min.js"
wget -x "https://cdn.jsdelivr.net/npm/anchor-js@5.0.0/anchor.min.js"
wget -x "https://cdn.jsdelivr.net/npm/mermaid@9.1.3/dist/mermaid.min.js"
wget -x "https://cdn.jsdelivr.net/gh/osano/cookieconsent@3.1.1/build/cookieconsent.min.js"
wget -x "https://cdn.jsdelivr.net/gh/plotly/plotly.js@v1.55.2/dist/plotly.min.js"
wget -x "https://cdn.jsdelivr.net/gh/jpswalsh/academicons@1.9.4/css/academicons.min.css"
wget -x "https://cdn.jsdelivr.net/npm/leaflet@1.7.1/dist/leaflet.min.css"
wget -x "https://cdn.jsdelivr.net/npm/@fancyapps/fancybox@3.5.7/dist/jquery.fancybox.min.css"
wget -x "https://cdn.jsdelivr.net/npm/instantsearch.css@7.4.5/themes/satellite-min.css"
wget -x "https://cdn.jsdelivr.net/gh/osano/cookieconsent@3.1.1/build/cookieconsent.min.css"
```

然后将这个文件中的所有 `https://` 替换为 `/local/`

### 修改 fonts.googleapis.com css2文件

将这个css文件保存到 `static/local/fonts.googleapis.com` 目录下，改名为 css2.css: 

```bash
https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&family=Roboto+Mono&family=Roboto:wght@400;700&display=swap 
```

修改 `modules/blox-bootstrap/layouts/partials/site_head.html` 文件，将原来的内容从：

```html
      <link rel="preload" as="style" {{ printf "href=\"https//fonts.googleapis.com/css2?%s&display=swap\"" . | safeHTMLAttr }}>
      <link rel="stylesheet" {{ printf "href=\"https//fonts.googleapis.com/css2?%s&display=swap\"" . | safeHTMLAttr }} media="print" onload="this.media='all'">
```

修改为

```html
      <link rel="preload" as="style" {{ printf "href=\"/local/fonts.googleapis.com/css2.css?%s&display=swap\"" . | safeHTMLAttr }}>
      <link rel="stylesheet" {{ printf "href=\"/local/fonts.googleapis.com/css2.css?%s&display=swap\"" . | safeHTMLAttr }} media="print" onload="this.media='all'">
```

这个修改方式可能会带来其他问题，因为参数可能不一样。但总比这个css地址老是被墙而无法访问好。

先这么对付用。

### 修改 woff2 文件

用开发者工具可以看到还是有部分 woff2 文件走的不是本地文件，比如：

https://fonts.gstatic.com/s/roboto/v30/KFOmCnqEu92Fr1Mu4mxKKTU1Kg.woff2

这些文件的地址定义在 上面的 fonts.googleapis.com css2.css 文件中。 grep 以下然后替换，就得到这些下载命令：

```bash
wget -x https://fonts.gstatic.com/s/montserrat/v26/JTUSjIg1_i6t8kCHKm459WRhyyTh89ZNpQ.woff2
wget -x https://fonts.gstatic.com/s/montserrat/v26/JTUSjIg1_i6t8kCHKm459W1hyyTh89ZNpQ.woff2
wget -x https://fonts.gstatic.com/s/montserrat/v26/JTUSjIg1_i6t8kCHKm459WZhyyTh89ZNpQ.woff2
wget -x https://fonts.gstatic.com/s/montserrat/v26/JTUSjIg1_i6t8kCHKm459WdhyyTh89ZNpQ.woff2
wget -x https://fonts.gstatic.com/s/montserrat/v26/JTUSjIg1_i6t8kCHKm459WlhyyTh89Y.woff2
wget -x https://fonts.gstatic.com/s/montserrat/v26/JTUSjIg1_i6t8kCHKm459WRhyyTh89ZNpQ.woff2
wget -x https://fonts.gstatic.com/s/montserrat/v26/JTUSjIg1_i6t8kCHKm459W1hyyTh89ZNpQ.woff2
wget -x https://fonts.gstatic.com/s/montserrat/v26/JTUSjIg1_i6t8kCHKm459WZhyyTh89ZNpQ.woff2
wget -x https://fonts.gstatic.com/s/montserrat/v26/JTUSjIg1_i6t8kCHKm459WdhyyTh89ZNpQ.woff2
wget -x https://fonts.gstatic.com/s/montserrat/v26/JTUSjIg1_i6t8kCHKm459WlhyyTh89Y.woff2
wget -x https://fonts.gstatic.com/s/roboto/v30/KFOmCnqEu92Fr1Mu72xKKTU1Kvnz.woff2
wget -x https://fonts.gstatic.com/s/roboto/v30/KFOmCnqEu92Fr1Mu5mxKKTU1Kvnz.woff2
wget -x https://fonts.gstatic.com/s/roboto/v30/KFOmCnqEu92Fr1Mu7mxKKTU1Kvnz.woff2
wget -x https://fonts.gstatic.com/s/roboto/v30/KFOmCnqEu92Fr1Mu4WxKKTU1Kvnz.woff2
wget -x https://fonts.gstatic.com/s/roboto/v30/KFOmCnqEu92Fr1Mu7WxKKTU1Kvnz.woff2
wget -x https://fonts.gstatic.com/s/roboto/v30/KFOmCnqEu92Fr1Mu7GxKKTU1Kvnz.woff2
wget -x https://fonts.gstatic.com/s/roboto/v30/KFOmCnqEu92Fr1Mu4mxKKTU1Kg.woff2
wget -x https://fonts.gstatic.com/s/roboto/v30/KFOlCnqEu92Fr1MmWUlfCRc4AMP6lbBP.woff2
wget -x https://fonts.gstatic.com/s/roboto/v30/KFOlCnqEu92Fr1MmWUlfABc4AMP6lbBP.woff2
wget -x https://fonts.gstatic.com/s/roboto/v30/KFOlCnqEu92Fr1MmWUlfCBc4AMP6lbBP.woff2
wget -x https://fonts.gstatic.com/s/roboto/v30/KFOlCnqEu92Fr1MmWUlfBxc4AMP6lbBP.woff2
wget -x https://fonts.gstatic.com/s/roboto/v30/KFOlCnqEu92Fr1MmWUlfCxc4AMP6lbBP.woff2
wget -x https://fonts.gstatic.com/s/roboto/v30/KFOlCnqEu92Fr1MmWUlfChc4AMP6lbBP.woff2
wget -x https://fonts.gstatic.com/s/roboto/v30/KFOlCnqEu92Fr1MmWUlfBBc4AMP6lQ.woff2
wget -x https://fonts.gstatic.com/s/robotomono/v23/L0xuDF4xlVMF-BfR8bXMIhJHg45mwgGEFl0_3vq_SeW4AJi8SJQtQ4Y.woff2
wget -x https://fonts.gstatic.com/s/robotomono/v23/L0xuDF4xlVMF-BfR8bXMIhJHg45mwgGEFl0_3vq_QOW4AJi8SJQtQ4Y.woff2
wget -x https://fonts.gstatic.com/s/robotomono/v23/L0xuDF4xlVMF-BfR8bXMIhJHg45mwgGEFl0_3vq_R-W4AJi8SJQtQ4Y.woff2
wget -x https://fonts.gstatic.com/s/robotomono/v23/L0xuDF4xlVMF-BfR8bXMIhJHg45mwgGEFl0_3vq_S-W4AJi8SJQtQ4Y.woff2
wget -x https://fonts.gstatic.com/s/robotomono/v23/L0xuDF4xlVMF-BfR8bXMIhJHg45mwgGEFl0_3vq_SuW4AJi8SJQtQ4Y.woff2
wget -x https://fonts.gstatic.com/s/robotomono/v23/L0xuDF4xlVMF-BfR8bXMIhJHg45mwgGEFl0_3vq_ROW4AJi8SJQt.woff2
```

将文件下载到 static/local 目录。

然后将这个文件中的所有 `https://` 替换为 `/local/`。

### 其他文件修改

```bash
# modules/blox-analytics/layouts/partials/blox-analytics/services/fathom.html
wget -x https://cdn.usefathom.com/script.js

# modules/blox-analytics/layouts/partials/blox-analytics/services/pirsch.html
wget -x https://api.pirsch.io/pirsch.js

# modules/blox-analytics/layouts/partials/blox-analytics/services/plausible.html
wget -x https://plausible.io/js/script.js
```

这些文件用开发者工具没有发现访问，算了，不继续改了。

### 小结

修改完上述的文件地址，基本上绝大部分的远程文件请求都被修改为本地文件请求了。
