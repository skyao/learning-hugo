---
title: "Google Analytics"
date: 2021-01-20
weight: 336
description: >
  启用Google Analytics来跟踪网站访问情况
---

### 创建Google Analytics账号

登录 Google Analytics 网站：

https://analytics.google.com/

创建账号和媒体，拿到 Analytics ID ，如 `G-4BVWTNS4MB`

### 设置Google Analytics支持

Docsy 内建对 Google Analytics 的支持。设置方式参考官方文档：

https://www.docsy.dev/docs/adding-content/feedback/

修改 config.toml 文件：

```toml
 [services.googleAnalytics]
 id = "G-4BVWTNS4MB"
```

### 构建时的特殊设置

> Ensure that your site is built with `HUGO_ENV="production"`, as Docsy only adds Analytics tracking to production-ready sites. 
>
> 确保网站构建时带有`HUGO_ENV="production"`，因为Docsy只在生产就绪的网站上添加分析跟踪。

需要在构建时为hugo增加环境变量：

```bash
$ env HUGO_ENV="production" hugo
```

