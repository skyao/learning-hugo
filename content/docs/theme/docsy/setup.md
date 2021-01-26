---
title: "搭建站点"
date: 2021-01-19
weight: 331
description: >
  搭建基于docsy主题的站点
---

{{% pageinfo %}}
git submodule方式在项目比较多时不方便，改用软链接
{{% /pageinfo %}}

### 安装hugo的extended版本

docsy主题和普通的hugo主题不同，需要使用到hugo的 extended 版本。具体安装方式请见前面的章节：

<a class="btn btn-lg btn-primary mr-3 mb-4" href="{{< relref "/docs/installation" >}}">安装 <i class="fas fa-arrow-alt-circle-up ml-2"></i></a>

### 准备example文件

将 docsy 主题文件的示例项目clone到本地：

```bash
git clone git@github.com:google/docsy-example.git
```

进入docsy目录下拉取 docsy 依赖的 submodule ：

```bash
cd docsy-example
git submodule update --init --recursive

子模组 'themes/docsy'（https://github.com/google/docsy）已对路径 'themes/docsy' 注册
正克隆到 '/Users/sky/work/code/learning2/docsy-example2/themes/docsy'...
Connection to github.com port 22 [tcp/ssh] succeeded!

子模组路径 'themes/docsy'：检出 '976ee59c947a153a5c4fe9adc620b32684505d73'
子模组 'assets/vendor/Font-Awesome'（https://github.com/FortAwesome/Font-Awesome.git）已对路径 'themes/docsy/assets/vendor/Font-Awesome' 注册
子模组 'assets/vendor/bootstrap'（https://github.com/twbs/bootstrap.git）已对路径 'themes/docsy/assets/vendor/bootstrap' 注册
正克隆到 '/Users/sky/work/code/learning2/docsy-example2/themes/docsy/assets/vendor/Font-Awesome'...
Connection to github.com port 22 [tcp/ssh] succeeded!

正克隆到 '/Users/sky/work/code/learning2/docsy-example2/themes/docsy/assets/vendor/bootstrap'...
Connection to github.com port 22 [tcp/ssh] succeeded!
子模组路径 'themes/docsy/assets/vendor/Font-Awesome'：检出 '951a0d011f8c832991750c16136f8e260efa60b5'
子模组路径 'themes/docsy/assets/vendor/bootstrap'：检出 'a716fb03f965dc0846df479e14388b1b4b93d7ce'
```

然后安装SCSS：

```bash
sudo npm install -D --save autoprefixer
sudo npm install -D --save postcss-cli
```

最后执行：

```bash
hugo server
```

用浏览器打开 http://localhost:1313/ 就可以看到效果了。

### 特别：软链接主题文件

我用 docsy 来作为学习笔记的主题，由于学习笔记有几十份之多（虽然很多笔记只有一点点内容），如果每个笔记都用 `git submodule` 机制，clone和更新的维护量太大。

因此我才用了为 docsy 主题文件创建软链接的方式，放弃 `git submodule` 机制，具体做法是：

1. 将 docsy 主题文件clone到本地：

   ```bash
   git clone git@github.com:google/docsy.git
   ```

2. 进入docsy目录下拉取 docsy 依赖的 submodule ：

   ```bash
   cd docsy
   git submodule update --init --recursive
   ```

     注意如果不拉取 submodule，在运行时会因为缺少文件而报错，如：

   ```bash
   Start building sites … 
   Built in 62 ms
   Error: Error building site: TOCSS: failed to transform "scss/main.scss" (text/x-scss): SCSS processing failed: file "stdin", line 6, col 1: File to import not found or unreadable: ../vendor/bootstrap/scss/bootstrap. 
   ```

3. 删除原有的 theme 目录和 .gitmodule 文件

   ```bash
   rm -rf themes
   rm .gitmodule
   ```

4. 创建软链接

   ```bash
   mkdir themes
   cd themes
   ln -s ../../docsy .
   ```

这样就可以让几十份学习笔记同时依赖一份 docsy 模版文件，当 docsy模版文件内容发生变化时，所有的学习笔记都可以借助于软链接而同时得到更新。

