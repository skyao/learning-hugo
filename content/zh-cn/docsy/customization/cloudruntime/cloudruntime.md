---
title: "为CloudRuntime网站特别定制"
linkTitle: "CloudRuntime定制"
weight: 100
description: >
  修改模版内容，为CloudRuntime网站特别定制
---



准备修改 docsy 和 docsy-example 仓库的内容，为CloudRuntime网站特别定制。

## 修改 docsy-example 仓库

### 准备分支

新建 cloudruntime 分支，为后续的各种网站准备 code base：

```bash
git checkout -b cloudruntime
git merge local-files
```

### 修改通用内容

#### 修改hugo.toml

##### 多语言设置

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
contentDir = "content/zh"
defaultContentLanguage = "zh-cn"
defaultContentLanguageInSubdir = false

......

# Language configuration

[languages]
[languages.en]
languageName ="English"
contentDir = "content/en"
# Weight used for sorting.
weight = 2
[languages.en.params]
title = "CloudRuntime"
description = "CloudRuntime site"
time_format_default = "2006.01.02"
time_format_blog = "2006.01.02"

[languages.zh-cn]
languageName ="Chinese"
contentDir = "content/zh"
# Weight used for sorting.
weight = 1
[languages.zh-cn.params]
title = "CloudRuntime"
description = "CloudRuntime 网站"
time_format_default = "2006.01.02"
time_format_blog = "2006.01.02"
```

默认语言修改为中文 zh-cn。

#### 复制中文内容

将 `content/en` 目录复制为 `content/zh` 目录，先重用英文内容为中文内容的基础，然后后续再修改为中文。

#### 修改为 CloudRuntime

```toml
title = "CloudRuntime"
url_latest_version = "https://cloudruntime.net"
github_repo = "https://github.com/cloudruntime/website"
github_project_repo = "https://github.com/cloudruntime/cloudruntime"
gcs_engine_id = "8762eb8456ad6400e"
# enable google analytics
[services.googleAnalytics]
  id = "G-RK2ZRH7XPE"

[params.ui.feedback]
enable = false
yes = 'Glad to hear it! Please <a href="https://github.com/cloudruntime/cloudruntime/issues/new">tell us how we can improve</a>.'
no = 'Sorry to hear that. Please <a href="https://github.com/cloudruntime/cloudruntime/issues/new">tell us how we can improve</a>.'

```



## 修改 docsy 仓库

### 准备分支

新建 cloudruntime 分支，为后续的各种网站准备 code base：

```bash
git checkout -b cloudruntime
git merge local-files
```

### 修改通用内容

#### 百度统计

修改 docsy 主题中的 `layouts/partials/footer.html` 文件，在 `<footer>` 标签中加入以下内容：

```javascript
<footer class="td-footer row d-print-none">
  <script>
    var _hmt = _hmt || [];
    (function() {
      var hm = document.createElement("script");
      hm.src = "https://hm.baidu.com/hm.js?b4f16bb2e57123f6faf09c8b3574d07a";
      var s = document.getElementsByTagName("script")[0]; 
      s.parentNode.insertBefore(hm, s);
    })();
    </script>    
</footer>
```

