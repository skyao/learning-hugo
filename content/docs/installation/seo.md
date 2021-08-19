---
title: "搜索引擎优化（待更新）"
date: 2021-01-18
weight: 240
description: >
  为了让Hugo网站更好的被搜索引擎收录，需要进行搜索引擎优化
---

## 站点内容优化

### 修改配置

修改 `hugo/config/_default` 目录下的 `params.toml` 文件：

```toml
description = "敖小剑的个人技术博客网站，主要关注服务网格,serverless,kubernetes,微服务等云原生技术。"
```

### 修改模板

0.54 版本下基本做的很好了，不再修改。

### 添加页面信息

首先确保每个页面一定都要设置有 title，description，最好还有 keywords：

```yaml
title: 前言
keywords:
- hugo
- 学习笔记
- hugo学习笔记
description : "介绍Hugo学习笔记的基本资料和访问方式。"
```

## google搜索优化

### 提交给Google网站站长

打开 [Google网站站长](https://www.google.com/webmasters/)，点击 "[SEARCH CONSOLE ](https://search.google.com/search-console?hl=zh-CN)" 进入，然后添加资源，如`https://skyao.io/learning-hugo/`。会要求下载一个html文件如`google571325××××.html`做验证，将这个文件保存到hugo站点根目录下的static子目录，更新站点内容让google search console可以访问到进行验证即可。

进入资源页面，点"索引"下的"站点地图"，在"添加新的站点地图"处输入当前hugo站点的sitemap，这个文件hugo会默认生成，就在根路径下，如`https://skyao.io/learning-hugo/sitemap.xml`。

## 百度搜索优化

打开 [百度搜索资源平台](https://ziyuan.baidu.com/) ，点击 [链接提交](https://ziyuan.baidu.com/linksubmit/index)，然后点"添加站点"。同样可以用文件验证的方式来进行网站验证。

进入"数据引入"下的"链接提交"，再点 "自动提交" 下的 "sitemap"，在这里可以提交hugo网站的sitemap文件。注意百度不容许以子目录的方式提交子站点，和google不一样，我们的学习笔记 `https://skyao.io/learning-hugo/` 就不能直接提交了，只能在提交sitemap文件时，提交多个sitemap文件。这样也能勉强让百度收录。

### Bing搜索优化

在 [Bing Webmaster Tools](https://www.bing.com/webmasters) 处用登录，然后选择导入站点。Bing webmaster tools支持直接从 google 导入数据，很方便。

操作界面和google很类似。

## 参考资料

有参考以下资料，特此鸣谢：

- [搜索引擎优化（SEO）](https://jimmysong.io/hugo-handbook/steps/seo.html): 来自宋静超的hugo handbook
- [Front-End-Checklist - Github](https://github.com/thedaviddias/Front-End-Checklist)
- [SEO 查询 - 站长之家](http://seo.chinaz.com/)
- [SEO Meta Tags](https://moz.com/blog/seo-meta-tags)
- [Meta Description](https://moz.com/learn/seo/meta-description)
- [从Hexo迁移到Hugo-送漂亮的Hugo Theme主题](http://www.flysnow.org/2018/07/29/from-hexo-to-hugo.html)
- [Hugo website SEO](https://keithpblog.org/post/hugo-website-seo/)
- [Hugo SEO Markup](https://gist.github.com/jeremyjaymes/403f1cb712d98e8c8a36c904055958d6)