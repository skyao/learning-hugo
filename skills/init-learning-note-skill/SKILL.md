---
name: init-learning-note-skill
description: 按 skyao 的 Hugo + Docsy 指南，从 learning-clean-2024 模板初始化新的 learning-* 学习笔记仓库，替换站点信息、配置搜索并验证构建。适用于新学习笔记的搭建，不用于研究正文或发布站点。
---

# 初始化学习笔记

把新的 `learning-<slug>` 仓库初始化为可以本地预览的 Hugo + Docsy 中文学习笔记，保留模板历史和配套构建方式。默认只建立首页和笔记入口；具体章节和研究正文由后续任务决定。

参考作者的 [创建新笔记指南](https://skyao.net/learning-hugo/docsy/customization/skyao-learning/create-new/)。需要安装环境时再阅读 [本地搭建指南](https://skyao.net/learning-hugo/docsy/customization/skyao-learning/setup/)。在线文章无法访问时，可以读取相邻 `learning-hugo/content/zh-cn/docsy/customization/skyao-learning/create-new.md`，并说明使用了本地版本。

## 识别目标与环境

从用户请求、工作目录和 `git remote -v` 推断以下参数；只询问无法可靠确定且影响结果的参数：

| 参数 | Personal Agent 示例 |
| --- | --- |
| 主题显示名 | `Personal Agent` |
| 仓库名 | `learning-personal-agent` |
| GitHub 仓库 | `https://github.com/skyao/learning-personal-agent` |
| 发布地址 | `https://skyao.net/learning-personal-agent/` |
| 中文标题 | `Personal Agent 学习笔记` |
| 描述 | `Personal Agent 学习笔记，记录学习个人智能体的过程和相关资料` |
| 独立搜索 ID | 用户提供或单独创建；不能从模板沿用 |

检查当前及父目录适用的 `AGENTS.md`、工作区改动、分支与提交、远程和可用模板引用。空仓库的 `git log` 报“没有提交”是正常状态；不要把它当成仓库损坏。已有笔记或用户改动时只补齐缺失初始化项，不覆盖内容，也不重复初始化。

配套环境通常是：

```text
learning/
├── docsy/          # skyao/docsy-learning，learning2024 分支
├── docsy-example/  # skyao/docsy-example-learning，build 分支提供脚本
└── learning-<slug>/
```

模板目前配套 Hugo **0.121.1 Extended**、Go、Node.js/npm。用 `hugo version` 确认 Extended 支持。`docsy.work` 引用 `../docsy/`；不要为初始化擅自升级 Hugo 或主题，也不要切换已经存在的共享主题目录的分支。只有环境缺失时才按指南安装依赖，不要先尝试全局安装。

生产构建需要从站点目录解析到 `postcss`、`postcss-cli`、`autoprefixer`。仅在相邻主题目录安装依赖不一定足够，可能出现 `npm exec postcss` 尝试联网、最终 Hugo `head-css.html` 超时的现象。按实际缺项在站点 `package.json` 声明依赖并安装，不要直接把模板超时报错当作无限递归。

## 导入模板

尚未创建仓库时，按用户指定的 owner/name 创建或克隆；已经在目标仓库时继续使用当前仓库。配套 `clone.sh` 用于尚不存在的本地目录，不要在已存在的目录重复运行。

`origin` 指向用户的笔记仓库。模板远程应为：

```bash
git remote add upstream git@github.com:skyao/docsy-example-learning.git
git fetch upstream
git merge upstream/learning-clean-2024
```

只在缺少 `upstream` 时添加；已存在时先核对地址，不能覆盖其他用途的远程。离线环境如果已有模板引用，可以直接合并，并记录其提交号和未联网刷新这一事实。采用合并保留模板历史，方便将来同步。空仓库可能直接得到模板提交；已有独立历史发生冲突时先检查原因，不默认加 `--allow-unrelated-histories`，不强制重置。实际命令受运行环境的权限约束，遇到权限错误走相应审批。

## 定制站点

以实际文件为准。指南列出旧路径 `config.toml`、`content/_index.md` 和 `content/docs/_index.md`；当前模板通常是 `hugo.toml`、`content/zh-cn/_index.md` 和 `content/zh-cn/docs/_index.md`。`config.yaml` 可能只用于最低 Hugo 版本检查，不要把站点设置重复写入其中。

按字段替换，而不是全局把 `xxx` 替换成同一字符串：

- `hugo.toml`：顶层标题、`languages.zh-cn.params` 中的标题与描述；`url_latest_version`、`github_repo`、`github_project_repo`；核对 `github_branch`。
- `README.md`：标题、主题简介、在线地址、源码链接、版权说明，以及实际可用的环境、预览、构建命令。
- 首页 `_index.md`：front matter、`blocks/cover` 标题和源码按钮；保留模板的封面、短代码及版式。
- 笔记入口 `docs/_index.md`：标题和描述，移除旧示例日期或改为初始化当天，添加简短入口说明。
- `package.json` 若带有示例站点元数据，更新 name、description、repository、homepage、bugs 等字段。保留兼容的依赖版本；若系统已有指定版本的 Hugo，可以移除 npm 的 `hugo-extended` 下载器依赖，避免其 postinstall 再下载二进制，并将这一选择写入 README。新空白站点可简化为实际有效的 build、build:production、serve 脚本，不把模板未实现的链接测试当作验证通过。

显示名可以带空格和大写；URL slug 使用仓库名。模板同时可能含 `xxx`、`xxxx`、`learning-xxx`、`learning-xxxx`，分别核对，避免生成 `learning-Personal Agent` 或残留多余的 `x`。不把模板中尚未启用的 Hugo 示例 logo/favicon 当成主题的正式标志；需要换图时另行处理。

模板的 `baseURL = "/"` 与现有发布脚本按站点传 `--baseURL` 的方式配套。保留这一约定时，在 README 写清生产构建的完整地址；如用户指定固定 `baseURL`，以用户要求为准。保留继承代码的许可信息。

## 配置站内搜索

每个笔记应有独立的 Google Programmable Search Engine。在 [控制台](https://cse.google.com/cse/all) 创建的名称为仓库名，搜索范围为 `skyao.net/learning-<slug>/*`；生成代码中 `cx=` 的值就是 `gcs_engine_id`。

有该站点的 ID 时，将其写入 `hugo.toml` 的 `[params]` 区域中原字段位置。没有 ID 或无法操作 Google 控制台时，注释掉模板的 `gcs_engine_id`，在 README 记录引擎名称、范围和填入位置，然后继续完成可用的初始化。不要编造 ID、复用模板 ID，或把模板搜索误报成已配置。只有用户明确需要时再选择离线搜索。

注意 TOML 的字段归属：模板 `gcs_engine_id` 后面紧接 `[services.googleAnalytics]`。不要把新 ID 追加到文件末尾，否则可能落入其他表。

## 验证与交付

先审阅差异，再做一次实际 Hugo 构建。以下是生产子路径验证示例，实际目录使用本次任务独有的临时路径：

```bash
hugo --baseURL https://skyao.net/learning-<slug>/ --destination /tmp/learning-<slug>-build
```

自定义全新的 `--cacheDir` 会使 Hugo 重新下载模块；有可用缓存时不必重建。下载失败应区分网络限制、权限问题与模板错误，按运行环境要求重试，不通过升级主题来掩盖环境问题。

确认生成首页和 `/docs/` 入口，页面标题与仓库链接正确，站内链接和生成资源带预期的 `/learning-<slug>/` 前缀。主题还可能使用 `/local/` 根路径的共享资源；按现有发布约定单独核对，并检查本地预览能访问对应文件。扫描启用的配置和 Markdown 中的模板占位符，区分 skill 中的说明示例与实际站点残留。已启用搜索时，检查搜索页生成的 CSE 请求使用用户提供的 ID；搜索关闭时，检查导航没有搜索框、页面没有模板 ID 对应的 CSE 请求。静态构建不能证明 Google 已收录站点或在线搜索结果正常。

需要本地预览时，运行：

```bash
hugo server -D -F --disableFastRender --bind 127.0.0.1
```

构建成功不等于页面已经发布。交付时说明初始化结果、验证结果、预览方式以及是否还需提供搜索 ID。提交、推送和发布按用户已授权的范围执行，不把模板文章中的“可以提交”当成远程发布指令。
