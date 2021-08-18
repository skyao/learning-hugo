---
title: "[归档]2019年修改记录"
date: 2021-01-18
weight: 329
description: >
  详细介绍hugo的academic主题，使用方式和订制化
---

{{% pageinfo %}}
以下内容是2019年更新的，后续因为版本变动太大，已经没有参考价值，放在这里归档。
{{% /pageinfo %}}

## 主题介绍

academic是一个特别适合搭建内容相对比较丰富的网站的主题，如果我们hugo网站的内容不仅仅是博客，还有其他好几种样式的内容，那么academic会是一个很不错的选择。此外，academic主题简洁大方，也适合作为一个稍有规模的正式网站。

- 官网介绍：https://themes.gohugo.io/academic/
- 我用academic主题搭建的个人技术博客网站: https://skyao.io

## 准备工作

### git仓库准备

以建立skyao.io这个网站为例，fork github项目：

1. https://github.com/gcushen/hugo-academic: 修改仓库名为hugo-academic，这是自行订制的主题仓库，加cn后缀名以示区别。
2. https://github.com/sourcethemes/academic-kickstart: 修改仓库名为skyao.io，这是存放站点内容的仓库，为了方便起见，从官方的kickstart仓库开始改起，也方便未来保持更新。

> 备注：实际证明，academic的版本变化非常大，fork出来之后，再修改，到升级版本时到处是冲突，极易出错，很难正确处理。最后还是不得不从头来过：取到最新版本，然后手工将原有的改动重新再做一遍。

kickstart 项目就没有必要再fork了，hugo-academic 还是需要 fork 的。

### 本地仓库准备

clone下来 kickstart 的仓库到本地：

```bash
# 本地准备好academic主题仓库
git clone https://github.com/skyao/hugo-academic.git
# 直接获取kickstart的内容作为建站的基础
git clone https://github.com/sourcethemes/academic-kickstart.git skyao.io
cd skyao.io/
rm -rf .git .gitmodules
rm -r themes/academic/
```

修改`.gitignore`文件内容如下：

```bash
.*
!.gitignore
public/
themes/
```

修改`update_academic.sh`文件内容如下：

```bash
#!/bin/bash

if [ ! -d "themes" ];then
  echo "No themes directory, create it"
  mkdir themes
fi

if [ -d "themes/academic" ];then
  echo 'Find directoy "themes/academic", update by "git pull"'
  cd themes/academic
  git pull
  cd ../../
else
  echo 'Directoy "themes/academic" not found, do "ln -s"'
  cd themes
  ln -s ../../hugo-academic academic
  cd ../
fi
```

执行命令 `sh update_academic.sh` 获取 academic 主题文件(实际是一个ln过程)。此时`themes/academic`是我们订制的主题内容，此时两个git仓库都可以分别提交内容，而且实时生效，非常方便本地修改。

## 创建网站

### 使用 exampleSite 初始化

删除｀skyao.io｀目录下的config.toml文件和content/static目录，然后从｀themes/academic/exampleSite｀下的这三个文件和目录复制到｀skyao.io｀目录下。执行`hugo server`命令，就可以通过浏览器访问 http://localhost:1313/ 看到示例站点。

我们从这个基础开始进行修改和订制。

## 修改配置

注意新版本的hugo将原来的单个config.toml配置文件拆分为多个配置文件，这些配置文件在 config/_default 目录下。

### 修改config.toml文件

```toml
title = "敖小剑的博客"
copyright = "skyao.io &copy; 2019"
googleAnalytics = "17048c9d4209e5d08a9ae5b0b4160808"

# 修改默认语言，需要对应的在languages.toml添加中文
defaultContentLanguage = "zh"
hasCJKLanguage = true

paginate = 20
```

### 修改languages.toml

先只替换默认语言为中文，暂时不启用多语言版本。

```toml
# 注释掉en
#[en]
#  languageCode = "en-us"

# 添加中文
[zh]
  languageCode = "zh-Hans"
```

如果要调整页面上的字眼，需要修改主题文件中的 i18n 文件，如`themes/academic/i18n/zh.yaml`。

如果要修改 publication_types 的显示，需要参考 `themes/academic/layouts/partials` 下 pub_types.html 的内容，在 `themes/academic/i18n/zh.yaml` 文件中加入对应的内容：

```toml
- id: pub_uncat
  translation: 未分组
- id: pub_conf
  translation: 会议记录
- id: pub_journal
  translation: 期刊文章
- id: pub_manuscript
  translation: 手稿
- id: pub_report
  translation: 技术报告
- id: pub_book
  translation: 书籍
- id: pub_book_section
  translation: 书籍摘抄
```



### 修改params.toml

```toml
color_theme = "1950s"

description = "敖小剑的个人技术博客网站，主要关注服务网格,serverless,kubernetes,微服务等云原生技术。"

# 这个还不知道该如何设置
sharing_image = ""

twitter = "SkyAoXiaojian"

date_format = "2006-01-02"
time_format = "15:04"

email = "aoxiaojian@hotmail.com"
phone = ""
address = "广州市天河区广电平云广场B塔15楼"
office_hours = ""

contact_links = [
  {icon = "twitter", icon_pack = "fab", name = "DM Me", link = "https://twitter.com/SkyAoXiaojian"},
  # {icon = "skype", icon_pack = "fab", name = "Skype Me", link = "skype:echo123?call"},
  # {icon = "keybase", icon_pack = "fab", name = "Chat on Keybase", link = "https://keybase.io/"},
  # {icon = "comments", icon_pack = "fas", name = "Discuss on Forum", link = "https://discourse.gohugo.io"},
  # {icon = "telegram", icon_pack = "fab", name = "Telegram Me", link = "https://telegram.me/@Telegram"},
  ]
  
  reading_time = false
  comment_count = false
  
  [publications]
  date_format = "2006-01-02"
```

### 修改menus.toml

```toml
[[main]]
  name = "首页"
  url = "#about"
  weight = 1

[[main]]
  name = "演讲分享"
  url = "/talk/"
  weight = 2

[[main]]
  name = "出版作品"
  url = "/publication/"
  weight = 3

[[main]]
  name = "技术博客"
  url = "/post/"
  weight = 4

[[main]]
  name = "开源项目"
  url = "#projects"
  weight = 5

[[main]]
  name = "学习笔记"
  url = "/learning/"
  weight = 6

[[main]]
  name = "内容标签"
  url = "#tags"
  weight = 7

[[main]]
  name = "和我联系"
  url = "#contact"
  weight = 8
```

## 订制首页

需要修改的文件在 content/home 目录下。

关闭部分不需要的内容（设置active = false）：

- accomplishments.md
- experience.md
- skills.md
- teaching.md

### about.md

```toml
title = "简介"
```

然后修改 author/admin 下的文件，如替换头像，修改_index.md:

```toml
name = "敖小剑"
role = "中年码农"
organizations = [ { name = "Dreamfly.io", url = "https://dreamfly.io" } ]
bio = "我目前研究的方向主要在微服务/Microservice、服务网格/Servicemesh、无服务器架构/Serverless等和云原生/Cloud Native相关的领域，欢迎交流和指导"
email = "aoxiaojian@hotmail.com"
interests = [
    "微服务/Micro Service",
    "服务网格/Service Mesh",
    "无服务器架构/Serverless",
    "云原生/Cloud Native",
    "敏捷/DevOps"
]

[[education.courses]] # 删除

## 图标的代码需要在 https://fontawesome.com 网站上查找
[[social]]
  icon = "envelope"
  icon_pack = "fas"
  link = "mailto:aoxiaojian@hotmail.com"  # For a direct email link, use "mailto:test@example.org".

[[social]]
  icon = "twitter"
  icon_pack = "fab"
  link = "https://twitter.com/SkyAoXiaojian"

[[social]]
  icon = "github"
  icon_pack = "fab"
  link = "https://github.com/skyao"

[[social]]
  icon = "git"
  icon_pack = "fab"
  link = "//legacy.gitbook.com/@skyao"

[[social]]
  icon = "weibo"
  icon_pack = "fab"
  link = "//weibo.com/aoxiaojian"

[[social]]
  icon = "youtube"
  icon_pack = "fab"
  link = "//www.youtube.com/channel/UCKeIYzzIeOtVR1iSfAdRBqQ"
  
  
增加个人简介
```

### hero.md

暂时 禁用，后面再来设置

```toml
title = "SOFAMesh"
overlay_img = "projects/sofamesh-wide.jpg" # 记得把图片放到static/img/下

# 其他内容酌情修改
```

hero_carousel.md 是另一个版本的hero，横屏，可以多个话题滑动，效果更好的感觉。后面再整理。

### posts.md

```toml
title = "技术博客"
count = 10

view = 2
```

修改 content/posts/_index.md 文件：

```toml
title = "技术博客"
view = 3
```

### publications.md

```toml
title = "出版作品"
count = 5
weight = 30

view = 3
exclude_featured = true
```

修改 content/publications/_index.md 文件：

```toml
title = "出版作品"
view = 3
```

### publications_featured.md

```toml
title = "特别推荐"
```

### tags.md

```toml
title = "内容标签"
```

### projects.md

```toml
title = "开源项目"
```

### contact.md

```toml
title = "和我联系"
```

### talks.md

```tom
title = "演讲分享"
count = 5
weight = 20
```

修改talks_selected.md:

```tom
title = "下一站"
subtitle = "近期活动，欢迎关注"
```


### 修改post

修改 content/posts/_index.md

```to
title = "技术博客"

# 在页面 http://localhost:1313/post/ 显示一张图片
[header]
image = ""
caption = ""
```

### 修改project

project没有要修改的内容。 

### 修改publications

修改 content/publication/_index.md

```tom
title = "最近发表"
[header]
image = ""
caption = ""
```

### 增加 learning

新版本提供了 content/tutorials 的实例，不过没有挂在首页，不容易发现。

官方说明是适合用于：

- 项目或者软件文档
- 在线课程
- 教程

我这次用它来记录我的学习笔记，因为数量繁多不好整理，一直都没有放在个人网站上。

具体修改内容如下：

1. 重命名 `content/tutorials` 为 `content/learning`，通过 http://localhost:1313/learning/ 可以访问

2. 由于目录被改名，所以左边的菜单报错，修改修改`example.md`中

	```toml
	[menu.learning]	# 将menu.tutorial修改为menu.learning
	  parent = "Example Topic"
	  weight = 1
	```

	修改`_index.md`:

	```toml
	[menu.learning] # 将menu.tutorial修改为menu.learning
	  name = "Overview"
	  weight = 1
	```

	菜单恢复正常

3. 在顶部的导航条中增加链接

	修改`config.toml`，增加菜单：

	```toml
	[[menu.main]]
	  name = "学习笔记"
	  url = "/learning/"
	  weight = 5
	```

4. 修改`themes/academic/layouts/partials/docs_layout.html`，注释掉一下内容：

	```html
	<!--
	<div class="body-footer">
	{{ i18n "last_updated" }} {{ $.Lastmod.Format $.Site.Params.date_format }}
	</div>
	-->
	```

	不直接显示最后修改时间在页面上，这样界面清爽一些。

### slides功能

在content/slides下发现一个 example-slides.md 文件，用浏览器打开下面的地址：

http://localhost:1313/slides/example-slides/

发现原来是一个用markdown书写slides的功能，很有意思，稍后研究。

### 图标和其他文件设置

复制 icon.png / icon-192.png 到 static/img 目录下。

复制 baidu_verify_××××.html 和 google××××.html 到 static 目录下。

## 优化

### 关闭google地图

首页的google地图现实，太影响页面打开速度。最后选择关闭。修改config.toml:

```toml
  map = 0 # 最后还是决定关闭地图现实，太影响页面打开速度
```

### 去掉github项目的star显示

在hero页面上，默认设置会去取github项目的star数量，同样非常的拖累整体速度。还是去掉吧，修改文件`content/home`，删除以下内容：

```html
  <a id="academic-release" href="https://github.com/alipay/sofa-mesh/releases" data-repo="alipay/sofa-mesh">
  Latest release <!-- V -->
  </a>
<div class="mt-3">
  Star
</div>
<script async defer src="/local/buttons.github.io/buttons.js"></script>
```

### 改google analytics为百度统计

修改 `academic/layouts/partials/header.html`，将下面原来用于google analytics的内容：

```go
  {{ if not .Site.IsServer }}
  {{ if .Site.GoogleAnalytics }}
    <script>
      window.ga=window.ga||function(){(ga.q=ga.q||[]).push(arguments)};ga.l=+new Date;
      ga('create', '{{ .Site.GoogleAnalytics }}', 'auto');
      {{ if .Site.Params.privacy_pack }}ga('set', 'anonymizeIp', true);{{ end }}
      ga('require', 'eventTracker');
      ga('require', 'outboundLinkTracker');
      ga('require', 'urlChangeTracker');
      ga('send', 'pageview');
    </script>
    <script async src="//www.google-analytics.com/analytics.js"></script>
    {{ if ($scr.Get "use_cdn") }}
    {{ printf "<script async src=\"%s\" integrity=\"%s\" crossorigin=\"anonymous\"></script>" (printf $js.autotrack.url $js.autotrack.version) $js.autotrack.sri | safeHTML }}
    {{ end }}
  {{ end }}
  {{ end }}
```

修改为百度统计的内容：

```toml
  {{ if not .Site.IsServer }}
  {{ if .Site.GoogleAnalytics }}
	<script>
	var _hmt = _hmt || [];
	(function() {
	  var hm = document.createElement("script");
	  hm.src = "https://hm.baidu.com/hm.js?{{ .Site.GoogleAnalytics }}";
	  var s = document.getElementsByTagName("script")[0];
	  s.parentNode.insertBefore(hm, s);
	})();
	</script>
  {{ end }}
  {{ end }}
```

### 本地文件加速

使用中，发现网页装载速度不是很好，而且有时无法访问。检查后发现问题出现在页面上的一些静态文件下载上，使用的路径居然是`https://cdnjs.cloudflare.com/××`。

```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha512-6MXa8B6uaO18Hid6blRMetEIoPqHf7Ux1tnyIQdpt9qI5OACx7C+O3IVTr98vwGnlcg0LOLa02i9Y1HpVhlfiw==" crossorigin="anonymous">
```

cloudflare网站是在国外，访问速度慢，而且非常不稳定：经常被墙。导致页面加载的速度慢，有时还会出现无法打开页面的问题（被墙）。解决的方式就是将这些文件放在网站本地，从而避开这个问题。

具体做法：

1. 修改 `themes/academic/data/assets.toml`，将类似`https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js` 的地址修改为 `/local/cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js`
2. 修改 `fonts.googleapis.com/css.css` 文件，用 `/local/fonts` 替换 `https://fonts` ，将原来访问`https://fonts.gstatic.com`的地址指到本地。
3. 修改 `themes/academic/` 下的文件，将类似 `//cdnjs.cloudflare.com` 的地址替换为 `/local/cdnjs.cloudflare.com`
	- layouts/partials/header.html
	- layouts/partials/footer.html
	- layouts/partials/cookie_consent.html
	- layouts/slides/baseof.html
4. 然后将上述这些地址所指向的文件存放在`static/img/local`目录下，最大的工作量在这里。

最终的目标是实现下面第一个地址修改为第二个地址。

- https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js
- https://127.0.0.1:1313/local/cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js

小技巧：

1. 用chrome浏览器的开发工具，看network，就知道有哪些文件是走remote访问了
2. `find -type f -exec grep -H "//cdn" {} \;` 这样的命令可以方便的查找存在问题的文件


## 主题模板调整

0.54 版本的 academic 主题，在 post、publication、talk 等几个列表显示时，都不再提供图片显示，效果和之前版本差别好大，因此考虑修改模板。

`academic/layouts/partials/widgets`

### talk

修改 `themes/academic/layouts/partials/talk_li_card.html` 文件。

下面的内容，只显示日期，不显示具体时间。然后显示publication信息

```html
  <div class="talk-metadata" itemprop="startDate">
    {{ $date := .Params.time_start | default .Date }}
    {{ (time $date).Format $.Site.Params.date_format }} @ {{ .Params.publication }}
  </div>
```

原有的这段内容删除，非常难看：

```html
{{ (time $date).Format $.Site.Params.date_format }}
    {{ if not .Params.all_day }}
      {{ (time $date).Format ($.Site.Params.time_format | default "3:04 PM") }}
      {{ with .Params.time_end }}
        &mdash; {{ (time .).Format ($.Site.Params.time_format | default "3:04 PM") }}
      {{ end }}
    {{ end }}
```

删除一下内容，新版本的 featured image 的显示丑的没法忍：

```html
  {{ $resource := (.Resources.ByType "image").GetMatch "*featured*" }}
  {{ $anchor := .Params.image.focal_point | default "Smart" }}
  {{ with $resource }}
  {{ $image := .Fill (printf "918x517 q90 %s" $anchor) }}
  <div>
    <a href="{{ $.RelPermalink }}">
      <img src="{{ $image.RelPermalink }}" class="article-banner" itemprop="image" alt="">
    </a>
  </div>
  {{end}}
```

将旧版本的横幅图片显示搬回来，在上面位置加入如下内容：

```html
  {{ if .Params.image_preview }}
    {{ .Scratch.Set "image" .Params.image_preview }}
  {{ else if .Params.header.image }}
    {{ .Scratch.Set "image" .Params.header.image }}
  {{ end }}
  {{ if .Scratch.Get "image" }}
  <div>
    <a href="{{ .RelPermalink }}">
      {{ $img_src := urls.Parse (.Scratch.Get "image") }}
      {{ if $img_src.Scheme }}
        <img src="{{ .Scratch.Get "image" }}" class="article-banner" itemprop="image">
      {{ else }}
        <img src="{{ "/img/" | relURL }}{{ .Scratch.Get "image" }}" class="article-banner" itemprop="image">
      {{ end }}
    </a>
  </div>
  {{ end }}
```

### publication

修改 `themes/academic/layouts/partials/publication_li_card.html` 文件。

同样去掉featured images的代码，也同样将原来的横幅图片显示搬回来。

然后作者信息在最上面，有些不好看，将这行移动到下面一点的位置：

```html
{{ partial "page_metadata" (dict "content" $ "is_list" 1) }}
```

### post

修改 `themes/academic/layouts/partials/post_li_card.html` 文件。

同样去掉featured images的代码，也同样将原来的横幅图片显示搬回来。

### project

暂时还不清楚如何设置，后面再弄。

### leanging

学习笔记中，0.54 版本不再显示 header images了，改回来。

需要修改 `themes/academic/layouts/partials/doc_layout.html` 文件 ，加入 

`{{ partial "header_image.html" . }}` 这一行代码：

```html
      <article class="article" itemscope itemtype="http://schema.org/Article">

        {{ partial "header_image.html" . }}

        <div class="docs-article-container">
```

然后将老版本的 header_image.html 文件复制到 themes/academic/layouts/partials/ 目录下。

