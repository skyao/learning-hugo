---
title: "为个人技术博客网站特别定制"
linkTitle: "博客网站定制"
weight: 30
description: >
  修改模版内容，为个人技术博客网站特别定制
---

修改 theme-academic-cv 仓库[ ](http://localhost:1313/docsy/customization/skyao-learning/learning2024/#修改-docsy-example-仓库)

## 准备分支

使用之前新建 skyao-io-2024 分支，存放后续的修改：

```bash
git checkout -b skyao-io-2024
git push --set-upstream origin skyao-io-2024
```

## 修改配置

### 修改 languages.yaml

将默认语言修改为中文，暂时不需要支持多语言。

注释掉 en, 开启 zh:

```yaml
#en:
#  languageCode: en-us
  
zh:
  languageCode: zh-Hans
#  contentDir: content
  title: 敖小剑的博客
```

### 修改hugo.toml

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
baseURL: 'https://skyao.io/' # Website URL

# 分页数量从10修改为20
paginate: 20
```

### 修改 menus.yaml

这是默认的内容：

```yaml
main:
  - name: Home
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
  - name: Contact
    url: '#contact'
    weight: 60
```

修改为：

```yaml
main:
  - name: 首页
    url: '#about'
    weight: 10
  - name: 演讲分享
    url: talk/
    weight: 20
  - name: 出版作品
    url: publication/
    weight: 30
  - name: 技术博客
    url: post/
    weight: 40
  - name: 开源项目
    url: '#projects'
    weight: 50
  - name: 学习笔记
    url: learning/
    weight: 60
  - name: 内容标签
    url: '#tags'
    weight: 70
  - name: 和我联系
    url: '#contact'
    weight: 80
```

### 修改params.yaml

修改如下：

```yaml
# appearance

# 保持和原来的主题一致
appearance:
  theme_day: 1950s
  
# SEO

marketing:
  seo:
    description: '敖小剑的个人技术博客网站，主要关注服务网格,serverless,kubernetes,微服务等云原生技术。'
    twitter: 'SkyAoXiaojian'
  analytics:
    google_analytics: 'G-4BVWTNS4MB'
    baidu_tongji: '17048c9d4209e5d08a9ae5b0b4160808'
    
    
# Site footer

footer:
  copyright:
    notice: '© {year} skyao.io All Rights Reserved'
    license:
      enable: false
      share_alike: false # 这个暂时还不知道是显示什么内容
    
# Localization

locale:
  date_format: '2006-01-02'
  time_format: '15:04'
  address_format: en-us
  
  
# Site features

features:
  repository:
    url: 'https://github.com/skyao/skyao.io'
    content_dir: content
    branch: master
```

### 删除部分不需要的文件

- `data/page_sharer.toml`
- `images`
- preview.png

- `static/uploads/*`



## 首页内容修改

### 替换网站图标

替换 `assets/media/icon.png` 为自己的图标。

### 作者信息修改

修改 `content/author/admin` 目录下的 _index.md 文件，这些内容会直接显示在首页开头部分。

替换 avatar.jpg 文件为自己的头像。

### 去掉不需要的内容

- 删除 `content/privacy.md`
- 删除  `content/terms.md`
- 删除 `content/slides` 目录，暂时不用这个功能
- 删除 `content/event` 目录，暂时不用这个功能
- 

### 删除首页内容块

修改 `content/_index.md` 文件。

删除 experience /  accomplishments 这两个 block，这样首页就不会显示相关内容了。

```yaml
  - block: experience
  - block: accomplishments
```

删除 Gallery 图片显示，用不上：

```yaml
  - block: markdown
    content:
      title: Gallery
```

相关的图片存放在 `assets/media/albums/demo` 目录下，也可以一起删除。

### 修改 skills 模块的显示

skills 块被我修改为显示我关注的技术方向，但是新版本的样式非常不适合，因此我决定把老版本的内容搬回来。

老版本显示 skills 信息用过的是 `wowchemy/layouts/partials/widgets/featurette.html`，新版本用的是 `modules/blox-bootstrap/layouts/partials/blocks/skills.html` ，差别太大不适合直接改动。

好在新版本中还保留有 `modules/blox-bootstrap/layouts/partials/blocks/features.html` 这个 partial，和老版本的 featurette.html 非常类似，因此改用这个 features.html 来渲染内容。

修改 `content/_index.md` 文件，将 skills block的内容从原来的

```yaml
  - block: skills
     ......
```

替换为：

```yaml
  - block: features
    content:
      title: 重点关注
      text: '个人看好并正在努力学习和探索中的新技术方向'
      items:
        - name: App Runtime
          description: '为云原生应用提供全面支持'
          icon: object-group
          icon_pack: fa
        - name: Rust
          description: '云原生基础设施最佳编程语'
          icon: rust
          icon_pack: fab
        - name: Web Assembly
          description: '潜在的颠覆性技术突破点'
          icon: space-shuttle
          icon_pack: fa
```

### 定制首页节选的内容

首页可以选择从 post/publish/event 等内容中节选部分内容在首页上显示，这些节选的内容可以根据需要自行定制。

#### 最新分享

这是带 feartured 的 talk 内容：

```yaml
  - block: collection
    id: talks
    content:
      title: 最新分享
      subtitle: ''
      text: ''
      # Choose how many pages you would like to display (0 = all pages)
      count: 3
      # Filter on criteria
      filters:
        folders:
          - talk
        featured_only: true
      # Page order: descending (desc) or ascending (asc) date.
      order: desc
    design:
      # Choose a layout view
      view: compact
      columns: '2'
```

#### 演讲分享

这是不带 feartured 的 talk 内容：

```yaml
  - block: collection
    id: talks
    content:
      title: 演讲分享
      subtitle: ''
      text: ''
      # Choose how many pages you would like to display (0 = all pages)
      count: 2
      # Filter on criteria
      filters:
        folders:
          - talk
        exclude_featured: true
      # Page order: descending (desc) or ascending (asc) date.
      order: desc
    design:
      # Choose a layout view
      view: compact
      columns: '2'
```

#### 出版作品

publication 内容：

```yaml
  - block: collection
    id: publications
    content:
      title: 出版作品
      text: 
      # Choose how many pages you would like to display (0 = all pages)
      count: 1
      filters:
        folders:
          - publication
        exclude_featured: true
    design:
      columns: '2'
      view: citation
```

#### 最新博客 

Post 博客内容：

```yaml
  - block: collection
    id: latest-posts
    content:
      title: 最新博客
      subtitle: ''
      text: ''
      # Choose how many pages you would like to display (0 = all pages)
      count: 5
      # Filter on criteria
      filters:
        folders:
          - post
        exclude_featured: true
      # Page order: descending (desc) or ascending (asc) date.
      order: desc
    design:
      # Choose a layout view
      view: compact
      columns: '2'
```

#### 删除内容

删除最新事件的这段，有需要时再加回来。

```yaml
  - block: collection
    id: talks
    content:
      title: Recent & Upcoming Talks
      filters:
        folders:
          - event
    design:
      columns: '2'
      view: compact
```

其他类似的也删除。

#### 开源项目



```yaml
  - block: portfolio
    id: projects
    content:
      title: 开源项目
      filters:
        folders:
          - project
      # Default filter index (e.g. 0 corresponds to the first `filter_button` instance below).
      default_button_index: 0
      # Filter toolbar (optional).
      # Add or remove as many filters (`filter_button` instances) as you like.
      # To show all items, set `tag` to "*".
      # To filter by a specific tag, set `tag` to an existing tag name.
      # To remove the toolbar, delete the entire `filter_button` block.
      buttons:
        - name: 所有项目
          tag: '*'
        - name: 主导项目
          tag: own
        - name: 参与项目
          tag: participate
    design:
      # Choose how many columns the section has. Valid values: '1' or '2'.
      columns: '2'
      view: showcase
      # For Showcase view, flip alternate rows?
      flip_alt_rows: true
```



#### 内容标签

```yaml
  - block: tag_cloud
    id: tags
    content:
      title: 内容标签
    design:
      columns: '2'
```

#### 和我联系

简化内容：

```yaml
  - block: contact
    id: contact
    content:
      title: 和我联系
      subtitle:
      text: |-
        欢迎和我取得联系，探讨技术问题。
      # Contact (add or remove contact options as necessary)
      email: aoxiaojian@hotmail.com
      phone: 
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
          link: 'https://twitter.com/skyaoxiaojian80'
      # Automatically link email and phone or display as text?
      autolink: true
      # Email form provider
    design:
      columns: '2'
```

删除地图显示，避免拖慢访问速度。修改 `config/_default/params.yaml` 文件，将 map.provider 设置为空即可。

```yaml
  map:
    provider: ''
    api_key: ''
    zoom: 15
```

### 去除 powered-by 信息

删除 powered-by 信息，让页面更简洁一些。

需要修改 hugo-blox-builder 仓库下的  `modules/blox-tailwind/layouts/partials/site_footer.html` 文件，删除这一段：

```yaml
  <p class="powered-by text-center">
    {{ $is_sponsor := site.Params.i_am_a_sponsor | default false }}
    {{ $hide_published_with_footer := site.Params.power_ups.hide_published_with | default true }}
    {{ if not (and $is_sponsor $hide_published_with_footer) }}
		.......
    {{ end }}
  </p>
```



## 主题模板调整

### 列表页面的图片显示

academic 主题，在 post、publication、talk 等几个列表显示图片时，会按照 `808*455` 的长宽进行处理，而我之前设置的图片都是长条形，因此显示效果和之前版本差别好大，不够美观，因此考虑修改模板，将图片预处理的长宽比修改为 `808*210` （等比1920x500计算而来）。

修改 `modules/blox-bootstrap/layouts/partials/views/card.html`，将原有的

```html
  {{ $image := .Fill (printf "808x455 %s" $anchor) }}
```

修改为：

```html
  {{ $image := .Fill (printf "808x210 %s" $anchor) }}
```

老版本这样修改就可以了，但是在新版本中这样修改无效了，需要继续修改 `modules/blox-bootstrap/assets/scss/wowchemy/elements/_content.scss` 文件中的 `.article-banner`，将高度都改成 210:

```css
.article-banner {
  width: 100%;
  height: 210px;
  object-fit: cover;

  @include media-breakpoint-up(lg) {
    height: 210px; /* Increased height on desktop */
  }
}
```



### 横幅图片的显示

academic 主题，在 post、publication、talk 等几个 single 页面显示 featured 图片时，是放在主题内容中的，宽度很有限。远没有将图片拉宽到占据整个页面宽度那样的视觉效果好。因此考虑修改过来。

从源代码中看到，图片可以设置多种摆放的方式，其中 默认 1 是和主体内容等宽。如果设置为3 ，则就可以实现占据整个页面宽度。

因此可以在每个需要改动的页面设置:

```toml
image:
  placement: 3
```

方便起见，可以在 _index.md 文件设置：

```toml
cascade:
- image:
    placement: 3
```

修改 `modules/blox-bootstrap/layouts/partials/page_header.html` ，将这一段的位置往下挪，让图片显示在最前面，这样图片宽度占据全屏宽度，就直接和导航条无缝连接了。

原有内容：

```yaml
{{ if ne $image.MediaType.SubType "gif" }}{{ $image = $image.Process "webp" }}{{ end }}

<div class="article-container pt-3">
  <h1>{{ $title }}</h1>

  {{ with $page.Params.subtitle }}
  <p class="page-subtitle">{{ . | markdownify | emojify }}</p>
  {{end}}

  {{ partial "page_metadata" (dict "page" $page "is_list" 0 "share" true) }}
  {{ partial "page_links_div.html" $page }}
</div>

{{/* Featured image */}}
<div class="article-header {{$image_container}} featured-image-wrapper mt-4 mb-4" style="max-width: {{$image.Width}}px; max-height: {{$image.Height}}px;">
  <div style="position: relative">
    <img src="{{ $image.RelPermalink }}" width="{{ $image.Width }}" height="{{ $image.Height }}" alt="{{ with $.Params.image.alt_text }}{{.}}{{ end }}" class="featured-image">
    {{ with $.Params.image.caption }}<span class="article-header-caption">{{ . | markdownify | emojify }}</span>{{ end }}
  </div>
</div>
{{else}}
```

修改为：

```yaml
{{ if ne $image.MediaType.SubType "gif" }}{{ $image = $image.Process "webp" }}{{ end }}

{{/* Featured image */}}
<div class="article-header {{$image_container}} featured-image-wrapper mt-4 mb-4" style="max-width: {{$image.Width}}px; max-height: {{$image.Height}}px;">
  <div style="position: relative">
    <img src="{{ $image.RelPermalink }}" width="{{ $image.Width }}" height="{{ $image.Height }}" alt="{{ with $.Params.image.alt_text }}{{.}}{{ end }}" class="featured-image">
    {{ with $.Params.image.caption }}<span class="article-header-caption">{{ . | markdownify | emojify }}</span>{{ end }}
  </div>
</div>

<div class="article-container pt-3">
  <h1>{{ $title }}</h1>

  {{ with $page.Params.subtitle }}
  <p class="page-subtitle">{{ . | markdownify | emojify }}</p>
  {{end}}

  {{ partial "page_metadata" (dict "page" $page "is_list" 0 "share" true) }}
  {{ partial "page_links_div.html" $page }}
</div>
{{else}}
```

图片的样式默认设置了 “mt-4 mb-4”，导致图片的上下都会留空白，这样顶部就不能和导航条融为一体了，因此删除 mt-4 ，顶部不留空白。将下面这行中的 mt-4 删除即可：

```yaml
{{/* Featured image */}}
<div class="article-header {{$image_container}} featured-image-wrapper mt-4 mb-4" style="max-width: {{$image.Width}}px; max-height: {{$image.Height}}px;">
```

### 不显示作者名

个人网站，所有的作者都是自己，没有必要到处显示了，不显示作者信息让页面保持简洁。

修改 `modules/blox-bootstrap/layouts/partials/page_metadata.html` ，删除下面这段内容：

```html
  {{/* If `authors` is set and is not empty. */}}
  {{ if $page.Params.authors }}
  {{ $authorLen := len $page.Params.authors }}
  {{ if gt $authorLen 0 }}
  <div>
    {{ partial "page_metadata_authors" $page }}
  </div>
  {{ end }}
  {{ end }}
```

### 统一显示阅读时长

默认只在 post 类型的页面上显示阅读时长，publication等是不显示的，感觉还是都显示比较好。

修改 `modules/blox-bootstrap/layouts/partials/page_metadata.html` ，将下面这段内容中的 post 判断删除：

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

### 删除出版作品页面的搜索功能

出版作品没多少，就不需要搜索了。修改 `modules/blox-bootstrap/layouts/section/publication.html` 文件，删除下面这一段内容：

```yaml
     <div class="form-row mb-4">
        <div class="col-auto">
          <input type="search" class="filter-search form-control form-control-sm" placeholder="{{ i18n "search_placeholder" }}" autocapitalize="off"
          autocomplete="off" autocorrect="off" role="textbox" spellcheck="false">
        </div>
        <div class="col-auto">
          <select class="pub-filters pubtype-select form-control form-control-sm" data-filter-group="pubtype">
            <option value="*">{{ i18n "publication_type" }}</option>
            {{ range $index, $taxonomy := site.Taxonomies.publication_types }}
            <option value=".pubtype-{{ $index }}">
              {{ i18n (printf "pub_%s" (strings.Replace $index "-" "_")) | default (strings.Title $index) }}
            </option>
            {{ end }}
          </select>
        </div>
        <div class="col-auto">
          <select class="pub-filters form-control form-control-sm" data-filter-group="year">
            <option value="*">{{ i18n "date" }}</option>
            {{ $years_sorted := $.Scratch.GetSortedMapValues "years" }}
            {{ if $years_sorted }}
            {{ range $year := sort $years_sorted "" "desc" }}
            <option value=".year-{{ $year }}">
              {{ $year }}
            </option>
            {{ end }}
            {{ end }}
          </select>
        </div>
      </div>
```

出版物类型之类的信息也没有必要显示，修改 `modules/blox-bootstrap/layouts/publication/single.html`，删除以下内容：

```yaml

    {{/* If the type is Uncategorized, hide the type. */}}
    {{ if $pub_type_display }}
    <div class="row">
      <div class="col-md-1"></div>
      <div class="col-md-10">
        <div class="row">
          <div class="col-12 col-md-3 pub-row-heading">{{ i18n "publication_type" }}</div>
          <div class="col-12 col-md-9">
            <a href="{{ (site.GetPage "section" "publication").RelPermalink }}#{{ $pub_type_csl | anchorize }}">
              {{ $pub_type_display }}
            </a>
          </div>
        </div>
      </div>
      <div class="col-md-1"></div>
    </div>
    <div class="d-md-none space-below"></div>
    {{ end }}

    {{ if .Params.publication }}
    <div class="row">
      <div class="col-md-1"></div>
      <div class="col-md-10">
        <div class="row">
          <div class="col-12 col-md-3 pub-row-heading">{{ i18n "publication" }}</div>
          <div class="col-12 col-md-9">{{ .Params.publication | markdownify }}</div>
        </div>
      </div>
      <div class="col-md-1"></div>
    </div>
    <div class="d-md-none space-below"></div>
    {{ end }}
```

### 页面内容宽度调整

默认页面正文内容的宽度最大为 760 像素，在今天4k显示器初步普及的现在有些小，页面上大片空白。因此考虑增加一些。

打开 `modules/blox-bootstrap/assets/scss/wowchemy/elements/_content.scss`，找到 `.article-container`:

```css
.article-container {
  max-width: 760px;
  padding: 0 20px;
  margin: 0 auto;
}
```

默认 760，增加到 960 比较合适。（尝试过 1280，文字内容太宽，图片太窄，有点不协调，退回960.）

## 更新网站

skyao.io 网站之前建立时是使用了其他的仓库，内容是调整之后再复制过去的，代码无法和 theme-academic-cv 仓库合并。

因此，在 theme-academic-cv 仓库代码修改完成之后，就需要将修改好的内容，复制到 skyao.io 网站的仓库，然后提交。

由于这个更新操作我一般2到3年才做一次，因此也还好。
