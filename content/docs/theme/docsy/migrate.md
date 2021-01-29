---
title: "迁移站点"
date: 2021-01-26
weight: 337
description: >
  迁移现有的学习笔记到docsy主题
---

{{% pageinfo %}}
为了日后能方便而干净的merge docsy主题的内容，做了很多工作
{{% /pageinfo %}}

## 背景

对 google 的 docsy 主题比较满意，尤其是看维护和更新的频率，应该未来起码两三年内不会出现如 material-docs 主题那样停止更新的惨剧。因此考虑长期使用这个主题并希望能及时更新。

为了实现这个目标，最理想的状态就是能通过 git merge 的方式方便的将最新的 docsy 主题的改动（包括我自己做的修改和调整）直接更新到数以几十记的学习笔记中。

实践中发现，这对 docsy /  docsy-example 和学习笔记的仓库都有要求

## 准备 docsy 仓库

从 https://github.com/google/docsy 仓库 fork 一个自己的 docsy

https://github.com/skyao/docsy

然后添加远程仓库，merge最新的master内容：

```bash
git remote add upstream git://github.com/google/docsy.git
git fetch --all
git merge upstream/master
git push
```

## 准备 docsy-example 仓库

通过准备 docsy-example 仓库的fork，由于需要对原仓库内容进行定制修改（默认中文并固定为只有中文，删除和学习笔记无关的内容，修改页面描述，本地化等各种定制），因此在master分支之外拉出来一个 learning 分支，专门用来存放用于学习笔记的模板。

实践中发现，如果直接把 docsy-example fork仓库的 learning 分支 merge 到学习笔记的仓库中（需要加 `--allow-unrelated-histories ` 参数强行merge），会导致学习笔记仓库下出现众多 docsy-example 仓库的提交记录，包括 contributor 列表，非常的混乱。

为了解决这个问题，使用暴力方案：

- 创建一个空白分支，命名 learning-clean
- 将 learning 分支下的内容复制到  learning-clean 分支并提交
- 以后如果 learning 分支有内容更新（包括官方改动和自己的改动），手工更新到 learning-clean 分支
- 学习笔记从 learning-clean 分支上merge

这个方案的好处是学习笔记的仓库可以保持干净，缺点就是需要手工维护  learning-clean 分支的更新，好在一般更新频率很低。

## 迁移学习笔记仓库

对学习笔记的仓库也有要求，主要是为了能够轻易的从 docsy-example 仓库的 learning-clean 分支上merge内容。

由于原有学习笔记已有git提交的历史记录，因此从 learning-clean 分支上merge内容必须用  `--allow-unrelated-histories ` 参数强行merge。考虑到学习笔记的提交历史记录不重要，因此我还是继续使用暴力方案：

- 在学习笔记下创建新的空白分支
- merge learning-clean 分支作为code base
- 复制学习比较的内容提交

这样之后就可以轻松的从  learning-clean 分支上以merge的形式更新内容了。由于学习笔记的内容基本都在content/目录下，重叠内容不多，不容易冲突。

具体步骤如下。

### 创建新的空白分支

```bash
git checkout --orphan docsy
git rm -rf .
ls -la
rm -rf *
ls -la
# 如果还有其他内容再 rm -rf 删除
```

这样就得到一个什么内容都没有的空白的分支。

### 添加docsy-example远程仓库

```bash
git add remote docsy git://github.com/skyao/docsy-example.git
git fetch --all
```

### merge learning-clean分支

```bash
git merge docsy/learning-clean
```

此时目录下就有从 `docsy-example` 仓库 `learning-clean` 分支上的干干净净的内容。

注意 merge 之前切记不要在空白分支上有任何的提交（commit），否则会报错:

```bash
git merge upstream/learning-clean
fatal: 拒绝合并无关的历史
```

虽然也可以加 `--allow-unrelated-histories ` 参数强行merge，但终究不美。

### 复制原笔记内容并提交

从之前备份的仓库中将学习笔记的内容复制过来，本地验证无误之后提交。

```bash
git push --set-upstream origin docsy
```

### 调整分支名称

在github的仓库下的 "settings" 页面的 "branches" 中，将默认 branch 从 master 修改为 docsy。

（否则不容许删除默认分支）

然后删除 master 分支，再将 docsy 分支重命名为 master 。之后本地仓库删除后重新 clone 一份新的。

至此，学习笔记的仓库提交记录就非常的干净，而且日后很容易一个简单的merge命令就更新了。代价就是牺牲了git的历史提交记录，但我不介意这些历史记录，更需要能方便的更新和保持仓库整洁。

## 创建新的学习笔记

新增学习笔记时，记得在仓库创建之后，不要在master分支上有任何内容（不要在创建时选择加入README文件），在做任何提交之前先从 docsy-example 仓库的 learning-clean 分支上 merge 内容，之后再提交学习笔记本身的内容。

## 总结

上述操作完成后，各个仓库都干干净净的，挺满意。

