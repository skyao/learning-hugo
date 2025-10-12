---
title: "scss报错文件找不到"
date: 2021-08-13
weight: 210
description: >
  scss报错 File to import not found or unreadable 的解决方式
---

### 问题描述

有时，在使用 docsy 的项目中，执行 hugo 命令时会遇到这样的报错：

```bash
$ hugo101 -D -F server --disableFastRender --bind "0.0.0.0" --port 4343

Start building sites … 
hugo v0.101.0-466fa43c16709b4483689930a4f9ac8add5c9f66+extended darwin/arm64 BuildDate=2022-06-16T07:09:16Z VendorInfo=gohugoio
Error: Error building site: TOCSS: failed to transform "scss/main.scss" (text/x-scss): "/Users/sky/work/code/dapr-cn/docsy/assets/scss/main.scss:6:1": File to import not found or unreadable: ../vendor/bootstrap/scss/bootstrap.
Built in 37 ms
```

问题发生的比较奇怪，因为之前还正常工作，内容也没有修改。git statu 没有文件改动，但就是报上述错误。

> 暂时只在 mac m1 max 上发现过一次。

文件夹目录如下：

```bash
$ ls
dapr-cn-site-source  docsy    
```

dapr-cn-site-source 中的 config.toml 文件设置为 :

```toml
[module]
  # uncomment line below for temporary local development of module
  replacements = "github.com/google/docsy -> ../../docsy"
```

### 解决方式

进入 docsy 目录，执行 `npm i bootstrap` 可以解决问题：

```bash
$ cd docsy
$ npm i bootstrap
npm WARN deprecated popper.js@1.16.1: You can find the new Popper v2 at @popperjs/core, this package is dedicated to the legacy v1

added 180 packages, and audited 181 packages in 27s

40 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

### 参考资料

google 之后找到这个文章，学习到 `npm i bootstrap` 命令解决问题：

- [javascript - Error: File to import not found or unreadable: ~bootstrap/scss/bootstrap - Stack Overflow](https://stackoverflow.com/questions/48434762/error-file-to-import-not-found-or-unreadable-bootstrap-scss-bootstrap)