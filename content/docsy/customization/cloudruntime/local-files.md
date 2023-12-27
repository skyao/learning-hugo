---
title: "使用本地文件加速网络访问"
linkTitle: "使用本地文件"
weight: 30
description: >
  使用本地文件加速，避免被墙
---



准备修改 docsy ，将可能会被墙的文件修改为使用本地文件。

为了方便修改，建议用 vs code 之类的 IDE 打开 fork 下来的 docsy 仓库。

## 准备local目录

在 docsy 仓库下建立 `static/local` 文件夹：

```bash
cd docsy
cd static
mkdir local
cd local
```

## 修改js文件

在 IDE 中 搜索 "https://", 然后用 wget 命令下载找到的各个文件中的各种网络文件，主要是js和css：

```bash
# layouts/partials/head.html
wget -x https://code.jquery.com/jquery-3.6.3.min.js
wget -x https://unpkg.com/lunr@2.3.9/lunr.min.js

# layouts/shortcodes/redoc.html
wget -x https://cdn.jsdelivr.net/npm/redoc@latest/bundles/redoc.standalone.js

# layouts/swagger/baseof.html
wget -x https://unpkg.com/swagger-ui-dist@5.1.0/swagger-ui.css
wget -x https://unpkg.com/swagger-ui-dist@5.1.0/swagger-ui-bundle.js
wget -x https://unpkg.com/swagger-ui-dist@5.1.0/swagger-ui-standalone-preset.js

# layouts/partials/scripts.html
wget -x https://cdn.jsdelivr.net/npm/markmap-autoloader
wget -x https://cdn.jsdelivr.net/npm/katex@0.16.4/dist/katex.min.css
wget -x https://cdn.jsdelivr.net/npm/katex@0.16.4/dist/katex.min.js
wget -x https://cdn.jsdelivr.net/npm/katex@0.16.4/dist/contrib/mhchem.min.js
wget -x https://cdn.jsdelivr.net/npm/katex@0.16.4/dist/contrib/auto-render.min.js
wget -x https://cdn.jsdelivr.net/npm/mermaid@9.3.0/dist/mermaid.min.js
wget -x https://cdn.jsdelivr.net/npm/@docsearch/js@3.5.2
```

比较麻烦的是 css 文件，暂时先不修改吧。

### 修改案例

以 jquery-3.6.3.min.js 文件为例，

```bash
cd docsy
cd layouts
find . -type f -exec grep -Hn 'jquery-3.6.3.min.js' {} \;

./layouts/partials/head.html:29:  src="https://code.jquery.com/jquery-3.6.3.min.js"
```

打开这个文件，找到 jquery-3.6.3.min.js ：

```html
<script
  src="https://code.jquery.com/jquery-3.6.3.min.js"
  integrity="sha512-STof4xm1wgkfm7heWqFJVn58Hm3EtS31XFaagaa8VMReCXAkQnJZ+jEy8PCC/iT18dFy95WcExNHFTqLyp72eQ=="
  crossorigin="anonymous"></script>
```

修改为 :

```html
<script
  src="/local/code.jquery.com/jquery-3.6.3.min.js"
  integrity="sha512-STof4xm1wgkfm7heWqFJVn58Hm3EtS31XFaagaa8VMReCXAkQnJZ+jEy8PCC/iT18dFy95WcExNHFTqLyp72eQ=="
  crossorigin="anonymous"></script>
```

此时再刷新页面，就能看到这个文件的地址被修改为：

http://localhost:1313/local/code.jquery.com/jquery-3.6.3.min.js 

## 修改css文件

部分css文件修改比较麻烦，单独列出来

### fonts.googleapis.com

首先是 `assets/scss/main.scss` 文件中会引出这个网络访问：

https://fonts.googleapis.com/css.css?family=Open+Sans:300,300i,400,400i,700,700i&display=swap

打开这个地址，将内容保存到文件 `static/local/fonts.googleapis.com/css.css` 中，

然后查找这个地址的出处，在这里：

```css
@if $td-enable-google-fonts {
  @import url($web-font-path);
}
```

定义在 `assets/scss/_variables.scss` 文件：

```css
$web-font-path: "https://fonts.googleapis.com/css?family=#{$google_font_family}&display=swap";
```

然后修改上面的内容为：

```css
$web-font-path: "/local/fonts.googleapis.com/css.css?family=#{$google_font_family}&display=swap";
```

继续修改这个保存下来的 fonts.googleapis.com/css.css 文件。

执行 `grep "wget -x https://" css.css`  命令，将地址提取出来，然后替换前后内容，得到这样一堆wget命令：

```
  # cd static/local
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWtE6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWvU6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWtU6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWuk6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWu06FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWxU6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqW106FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWtk6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWt06FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWuU6FxZCJgg.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWtE6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWvU6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWtU6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWuk6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWu06FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWxU6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqW106FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWtk6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWt06FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWuU6FxZCJgg.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWtE6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWvU6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWtU6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWuk6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWu06FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWxU6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqW106FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWtk6FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWt06FxZCJgvAQ.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memtYaGs126MiZpBA-UFUIcVXSCEkx2cmqvXlWqWuU6FxZCJgg.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSKmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSumu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSOmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSymu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTS2mu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTVOmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTUGmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSCmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSGmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTS-mu0SC55I.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSKmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSumu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSOmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSymu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTS2mu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTVOmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTUGmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSCmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSGmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTS-mu0SC55I.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSKmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSumu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSOmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSymu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTS2mu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTVOmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTUGmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSCmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSGmu0SC55K5gw.woff2
  wget -x https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTS-mu0SC55I.woff2
```

执行完成之后所有的文件就都保存到 local 文件夹下了。

再将这个 css.css 文件中所有的 `https//` 替换为 `/local/`，这样就会将

```css
  src: url(https://fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSGmu0SC55K5gw.woff2) format('woff2');

```

修改为：

```css
  src: url(/local/fonts.gstatic.com/s/opensans/v40/memvYaGs126MiZpBA-UvWbX2vVnXBbObj2OVTSGmu0SC55K5gw.woff2) format('woff2');
```

然后刷新页面，确认地址修改正确。

这个改动完成之后，css加载的速度就非常快了。

但这里有风险，google_font_family 现在相当于被写死为 `Open+Sans:300,300i,400,400i,700,700i`  了：

```bash
$web-font-path: "https://fonts.googleapis.com/css?family=#{$google_font_family}&display=swap";
```

后面如果遇到问题再调整。
