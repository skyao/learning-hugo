---
title: "创建一个新的学习笔记"
linkTitle: "创建新笔记"
weight: 70
description: >
  本地从头开始创建一个新的学习笔记的过沉。
---

简单记录备忘和快速重复搭建。

## 创建新仓库

创建新仓库，如 https://github.com/skyao/learning-debian

clone 到本地 learning 目录:

```bash
cd ~/work/code/learning
sh clone.sh learning-debian
```

## 初始化笔记

### 修改字眼

```bash
git merge upstream/learning-clean-2024
```

用 vscode 打开这个目录，然后搜索 "xxx"，替换为 "Debian" 或者 Debian :

- hugo.toml
- README.md
- content/_index.md
- content/docs/_index.md
- config.toml

### 修改 gcs_engine_id

https://cse.google.com/cse/all

创建一个新的搜索引擎：

- 名称： learning-debian
- 搜索内容： `skyao.io/learning-debian/*`


创建成功后的页面就会显示 

```html
<script async src="https://cse.google.com/cse.js?cx=a44bd639f3a554e52">
</script>
<div class="gcse-search"></div>
```

a44bd639f3a554e52 就是 gcs_engine_id

打开 config.toml，设置 gcs_engine_id。

```toml
# Google Custom Search Engine ID. Remove or comment out to disable search.
gcs_engine_id = "a44bd639f3a554e52"
```

此时就可以执行 `h` 命令查看效果了，没问题的话就可以提交，这样一个空白的学习笔记就创建出来了。后续填充内容即可。
