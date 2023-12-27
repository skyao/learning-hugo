---
title: "为学习笔记特别定制"
linkTitle: "学习笔记定制"
weight: 100
description: >
  修改模版内容，为学习比较特别定制
---



准备修改 docsy 和 docsy-example 仓库的内容，为CloudRuntime网站特别定制。

## 修改 docsy-example 仓库

### 准备分支

使用之前新建 learning2024 分支，为后续的各种学习准备 code base：

```bash
git checkout learning2024
```

### 修改内容

#### 修改hugo.toml

删除 no 和 fa 语言的内容：

```toml
[languages.no]
......
[languages.fa]
......
```

增加中文内容，网站只保留 en 和 zn-cn 两种语言，相关配置为：

```toml
# Language settings
contentDir = "content"
defaultContentLanguage = "zh-cn"
defaultContentLanguageInSubdir = false

......

# Language configuration

[languages]
# [languages.en]
# en 配置全部删除

[languages.zh-cn]
languageName ="中文"
# Weight used for sorting.
weight = 1
[languages.zh-cn.params]
title = "xxx学习笔记"
description = "xxx学习笔记，记录学习xxx的过程和相关资料"
contentDir = "content"
time_format_default = "2006.01.02"
time_format_blog = "2006.01.02"
```

学习笔记只保留中文。

其他修改：

```toml
enableGitInfo = false

[params]
copyright = "skyao.io"
privacy_policy = ""

url_latest_version = "https://skyao.io/learning-xxx"
github_repo = "https://github.com/skyao/learning-xxx"
github_project_repo = "https://github.com/skyao/learning-xxx"

gcs_engine_id = "d3c56aefaae284df1"
# enable google analytics
[services.googleAnalytics]
  id = "G-4BVWTNS4MB"

prism_syntax_highlighting = true
footer_about_disable = true

[params.ui.feedback]
enable = false
```

#### 修改通用页面内容

- 修改 `content/_index.md` 为学习笔记的页面。文字内容按照之前的学习笔记内容修改。这个页面是所有学习笔记都通用的。
- 修改 `featured-background.jpg` 为学习笔记的背景图片。
- 修改 `search.md` : 只修改一个 title 
- 将之前 `content/en` 目录下的 _index.md 复制到当前 `content` 目录。


修改完这些之后，一个学习笔记通用的模版内容就准备好了。

## 修改 docsy 仓库

docsy 仓库本来可以不用改了，但是 百度统计 这个功能最简单的办法就是修改 docsy 模版的 footer ，为了后续免的折腾就在这里修改吧。

### 修改通用内容

### 准备分支

使用 learning2024 作为学习笔记的定制化分支：

```bash
cd docsy
git checkout local-files
git checkout -b learning2024
git push --set-upstream origin learning2024
```

#### 百度统计

修改 docsy 主题中的 `layouts/partials/footer.html` 文件，在 `<footer>` 标签中加入以下内容：

```javascript
<footer class="td-footer row d-print-none">
<script>
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "https://hm.baidu.com/hm.js?17048c9d4209e5d08a9ae5b0b4160808";
  var s = document.getElementsByTagName("script")[0]; 
  s.parentNode.insertBefore(hm, s);
})();
</script>  
</footer>
```

