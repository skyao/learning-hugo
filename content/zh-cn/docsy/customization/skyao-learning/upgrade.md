---
title: "升级现有学习笔记"
linkTitle: "升级"
weight: 50
description: >
  记录升级现有学习笔记的过程，备用
---

背景：现有几十份学习笔记，需要更新到 leanring2024 分支的最新，这里记录更新的过程。

以 learning-hugo 为例。

## merge 最新内容

### merge learning-clean-2024 分支

```bash
cd learning2024
git clone git@github.com:skyao/learning-hugo.git
cd learning-hugo

git remote add upstream git@github.com:skyao/docsy-example-learning.git
git fetch --all

git merge upstream/learning-clean-2024 
```

### 解决 merge 冲突

需要处理 merge 的冲突文件：

```bash
CONFLICT (add/add): Merge conflict in README.md
CONFLICT (content): Merge conflict in config.toml
CONFLICT (content): Merge conflict in content/docs/_index.md
CONFLICT (content): Merge conflict in package.json
```

#### README.md

选择 "Accept Current Change" 保留当前仓库的内容。

#### config.toml

这个文件后面不会再使用，所以简单的选择 "Accept Current Change" 保留当前仓库的内容。

#### package.json

选择 "Accept Incoming Change" 更新本地仓库的内容。

#### content/docs/_index.md

选择 "Accept Current Change" 保留当前仓库的内容。

## 人工迁移内容

有几个文件的内容必须人工迁移。

### config.toml

这个文件在新版本中将被 hugo.toml 文件替代，所以里面的设置内容需要手工迁移到 hugo.toml 文件中。

需要修改的内容：

- 替换所有的 xxx 为 "hugo"

  ```toml
  title = "hugo学习笔记"
  title = "hugo学习笔记"
  description = "hugo学习笔记，记录学习hugo的过程和相关资料"
  
  url_latest_version = "https://skyao.net/learning-hugo"
  github_repo = "https://github.com/skyao/learning-hugo"
  github_project_repo = "https://github.com/skyao/learning-hugo"
  ```

- 修改 gcs_engine_id 和 googleAnalytics id

  ```toml
  gcs_engine_id = "8f763efbce07435e9"
  ```

  这个参数按说每个学习笔记都应该设置自己独有的 id。

  如果没有的话就用默认值  "d3c56aefaae284df1"，这是 `https://skyao.net` 整个网站的 gcs_engine_id。

- 修改 googleAnalytics id

  这个参数应该不用改。

  ```toml
  [services.googleAnalytics]
    id = "G-4BVWTNS4MB"
  ```

- 增加 plantuml 的参数

  ```toml
  [params.plantuml]
  enable = true
  theme = "default"
  #Set url to plantuml server 
  #default is http://www.plantuml.com/plantuml/svg/
  svg_image_url = "https://www.plantuml.com/plantuml/svg/"
  ```

  TBD: 直接修改 upstream/learning-clean-2024 ，增加这个内容。

### content/_index.html

这个文件在新版本中将被 _index.md 文件替代，所以里面的设置内容需要手工迁移到 _index.md 文件中。

需要修改的内容：

- 替换所有的 xxx 为 "hugo"

## 清理不再需要的内容

删除以下不再需要的文件：

- `config.toml`
- `content/_index.html`