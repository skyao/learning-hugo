---
title: "更新到2021年最新版本"
date: 2021-08-16
weight: 321
description: >
  更新到2021年最新版本，详细介绍hugo的academic主题的使用方式和订制化
---

{{% pageinfo %}}
2021年版本，从最新的 academic 主题开始更新个人博客网站。
{{% /pageinfo %}}

## 准备工作

### 准备自己的模版仓库

1. fork https://github.com/wowchemy/starter-hugo-academic 仓库到自己的 github 账号，不要用官方的 generated，那样会生成一个全新的仓库，以后就不好从上流更新内容了。
2. clone 到本地
3. 在本地从 master 分支处 checkout 一个 skyao. io 分支，以后所有的改动都在这个分支上进行

### 准备网站仓库

1. 复制 starter-hugo-academic 仓库下的 exampleSite 目录为新的文件夹 newsite，作为新站点的基础代码
2. 进入 newsite 目录，运行 hugo server ，即可启动 hugo ，看到网站

在这个目录下进行内容修改，等网站基本成型之后，在和原来的 skyao.io 网站内容融合，尽量保留原有仓库的历史提交记录。

## 配置网站

### 修改 languages.yaml

将默认语言修改为中文，暂时不需要支持多语言。

```yaml
# Default language
#en:
  #languageCode: en-us

zh:
  languageCode: zh-Hans
  title: 敖小剑的博客
```

### 修改config.yaml

将默认语言修改为中文:

```yaml
############################
## LANGUAGE
############################

defaultContentLanguage: zh
hasCJKLanguage: true
defaultContentLanguageInSubdir: false
```

其他修改：

```yaml
title: 敖小剑的博客 # Website name
baseurl: 'https://skyao.io/' # Website URL

# 分页数量从10修改为20
paginate: 20
```

### 修改 menus.yaml

这是默认的内容：

```yaml
main:
  - name: Demo
    url: '#about'
    weight: 10
  - name: Posts
    url: '#posts'
    weight: 20
  - name: Projects
    url: '#projects'
    weight: 30
  - name: Talks
    url: '#talks'
    weight: 40
  - name: Publications
    url: '#featured'
    weight: 50
  - name: Courses
    url: courses/
    weight: 60
  - name: Contact
    url: '#contact'
    weight: 70
```

修改为：

```yaml
main:
  - name: 首页
    url: '#about'
    weight: 10
  - name: 演讲分享
    url: '#talks'
    weight: 20
  - name: 出版作品
    url: '#featured'
    weight: 30
  - name: 技术博客
    url: '#posts'
    weight: 40
  - name: 开源项目
    url: '#projects'
    weight: 50
  - name: 学习笔记
    url: learning/
    weight: 60
  - name: Courses
    url: courses/
    weight: 60
  - name: 和我联系
    url: '#contact'
    weight: 70
```

### 修改params.yaml

Contact 信息修改如下：

```yaml
# Appearance
# 保持和原来的主题一致
theme: 1950s

# Contact (edit or remove options as required)

email: aoxiaojian@hotmail.com
address:
  street: 天河区
  city: 广州市
  region: 广东省
  postcode: '510600'
  country: 中国
  country_code: CN
contact_links:
  - icon: twitter
    icon_pack: fab
    name: DM Me
    link: 'https://twitter.com/SkyAoXiaojian'
```

### 删除部分不需要的文件

- `Netlify.toml`
- `static/media/*` 和 `static/uploads/*`

## 首页修改


### 去掉不需要的内容

- 删除 accomplishments.md
- 删除 experience.md
- 删除 hero.md
- 删除 demo.md
- 删除 hero-academic.png
- 删除 gallery.md
- 删除 privacy.md
- 删除 terms.md

关闭：

- demo.md

### 作者信息修改

修改 `content/author/admin` 目录下的 _index.md 文件，这些内容会直接显示在首页开头部分。

替换 avatar.jpg 文件为自己的头像。

### about.md

```yaml
title: 简介
```

其他信息在作者信息里面。

### 其他信息修改



## 主题修改

### fork 主题仓库

fork `github.com/wowchemy/wowchemy-hugo-modules` 仓库，clone 到本地。

后续需要有些需要是需要修改主题文件才能实现的。

在 Hugo 项目下，修改 go.mod 文件：

其原始内容是:

```go
require (
	github.com/wowchemy/wowchemy-hugo-modules/v5 v5.3.0
)
```

为了让这个依赖指向我们fork的仓库，需要增加 replace：

```
require (
	github.com/wowchemy/wowchemy-hugo-modules/v5 v5.3.0
)

replace github.com/wowchemy/wowchemy-hugo-modules/v5 => /Users/sky/work/code/skyao/hugo-academic
replace github.com/wowchemy/wowchemy-hugo-modules/wowchemy/v5  => /Users/sky/work/code/skyao/hugo-academic/wowchemy
```

执行命令 `hugo mod get -u` 更新 hugo 依赖的 module，之后就可以修改 fork 的主题文件仓库，然后在 hugo 项目这边运行 `hugo server` 就可以生效了。

> 参考 [Use Hugo Modules | Hugo (gohugo.io)](https://gohugo.io/hugo-modules/use-modules/)

### layout文件修改

`wowchemy/layouts/partials/site_footer.html`，修改footer，删除原来的内容，简单点。

```html
<!--
   <p class="powered-by">
     {{ $is_sponsor := site.Params.i_am_a_sponsor | default false }}
     {{ $hide_published_with_footer := site.Params.power_ups.hide_published_with | default true }}
@@ -49,4 +50,7 @@
       {{ $i18n_published_with | markdownify | emojify }}
     {{ end }}
   </p>
-->

<p class="powered-by">© 2021 skyao.io All Rights Reserved</p>
```

修改 `wowchemy/layouts/partials/book_layout.html` ，去掉最后修改时间的显示，页面简洁一些：

```html
        <div class="body-footer">
          <!--
          <p>{{ i18n "last_updated" }} {{ $.Lastmod.Format site.Params.date_format }}</p>
          -->
```

### 主题模板调整

### 列表页面的图片显示

academic 主题，在 post、publication、talk 等几个列表显示图片时，会按照 `808*455` 的长宽进行处理，而我之前设置的图片都是长条形，因此显示效果和之前版本差别好大，不够美观，因此考虑修改模板，将图片预处理的长宽比修改为  `808*210`   （等比1920x500计算而来）。

修改 `wowchemy/layouts/partials/li_card.html` 

```
  {{ $image := .Fill (printf "808x455 %s" $anchor) }}
  
  {{ $image := .Fill (printf "808x210 %s" $anchor) }}
```

在新版本中，如果需要显示图片，只需要将图片命名为 featured.jpg 或者 featured.png ，放在 md 文件同目录即可。不需要再在 md文件中设置 header image了。比较方便。

### 横幅图片的显示

academic 主题，在 post、publication、talk 等几个 single 页面显示 featured 图片时，是放在主题内容中的，宽度很有限。远没有将图片拉宽到占据整个页面宽度那样的视觉效果好。因此考虑修改过来。

从源代码中看到，图片可以设置多种摆放的方式，其中 默认 1  是和主体内容等宽。如果设置为3 ，则就可以实现占据整个页面宽度。

因此可以在每个需要改动的页面设置:

```toml
image:
  placement: 3
```

方便起见，可以在 _index.md 文件设置：

```toml
cascade:
- type: "publication"
- image:
    placement: 3
```

修改 `wowchemy/layouts/partials/page_header.html` ，将这一段的位置往下挪，让图片显示在最前面，这样图片宽度占据全屏宽度，就直接和导航条无缝连接了。

```html
<div class="article-container pt-3">
  <h1>{{ $title }}</h1>

  {{ with $page.Params.subtitle }}
  <p class="page-subtitle">{{ . | markdownify | emojify }}</p>
  {{end}}

  {{ partial "page_metadata" (dict "page" $page "is_list" 0 "share" true) }}
  {{ partial "page_links_div.html" $page }}
</div>
```

图片的样式默认设置了 "mt-4 mb-4"，导致图片的上下都会留空白，这样顶部就不能和导航条融为一体了，因此删除 mt-4 ，顶部不留空白：

```
<div class="article-header {{$image_container}} featured-image-wrapper mb-4" style="max-width: {{$image.Width}}px; max-height: {{$image.Height}}px;">
  <div style="position: relative">
    <img src="{{ $image.RelPermalink }}" alt="{{ with $.Params.image.alt_text }}{{.}}{{ end }}" class="featured-image">
    {{ with $.Params.image.caption }}<span class="article-header-caption">{{ . | markdownify | emojify }}</span>{{ end }}
  </div>
</div>
```

### 不显示作者名

个人网站，所有的作者都是自己，没有必要到处显示了，不显示作者信息让页面保持简洁。

修改 `wowchemy/layouts/partials/page_metadata.html` ，注释掉下面这段内容：

```html
  <!--
  {{/* If `authors` is set and is not empty. */}}
  {{ if $page.Params.authors }}
  {{ $authorLen := len $page.Params.authors }}
  {{ if gt $authorLen 0 }}
  <div>
    {{ partial "page_metadata_authors" $page }}
  </div>
  {{ end }}
  {{ end }}
  -->
```

### 统一显示阅读时长

默认只在 post 类型的页面上显示阅读时长，publication等是不显示的，感觉还是都显示比较好。

修改 `wowchemy/layouts/partials/page_metadata.html` ，将下面这段内容中的 post 判断删除：

```html
{{ if and (eq $page.Type "post") (not (or (eq site.Params.reading_time false) (eq $page.Params.reading_time false))) }}
<span class="middot-divider"></span>
<span class="article-reading-time">
  {{ $page.ReadingTime }} {{ i18n "minute_read" }}
</span>
{{ end }}
```

修改为：

```html
 
{{ if not (or (eq site.Params.reading_time false) (eq $page.Params.reading_time false)) }}
  <span class="middot-divider"></span>
  <span class="article-reading-time">
    {{ $page.ReadingTime }} {{ i18n "minute_read" }}
  </span>
  {{ end }}
```

如果不想显示阅读时长，可以在单独的页面上进行控制。

### 外观定制

更改网站图标，将图标 png 文件放在 `assets/media/` 目录下：

参考：https://wowchemy.com/docs/getting-started/customization/#website-icon

## 本地化加速

类似在 docsy 中的做法。