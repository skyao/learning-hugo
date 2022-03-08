---
title: "重来备忘录"
date: 2022-01-20
weight: 322
description: >
  每次重新安装系统之后都不得不从头来一次，为了方便，记录一下全过程以备参考:
---

## 常规流程


```bash
mkdir -p ~/work/code/skyao

# hugo-academic.git
cd ~/work/code/skyao
git clone git@github.com:skyao/hugo-academic.git
cd hugo-academic
git checkout skyao.io

# skyao.io.git
cd ~/work/code/skyao
git clone git@github.com:skyao/skyao.io.git
cd skyao.io

npm install -D --save autoprefixer
npm install -D --save postcss-cli

# run hugo server
h
```

> 备注：偶尔遇到git clone skyao.io 仓库时有文件意外损坏，导致build失败的情况，删除目录重新clone即可。

## m1上遇到的问题

遇到问题：

```bash
$ h              
WARN 2022/03/06 20:55:22 Module "github.com/wowchemy/wowchemy-hugo-modules/wowchemy/v5" is not compatible with this Hugo version; run "hugo mod graph" for more information.
Error: from config: failed to resolve output format "WebAppManifest" from site config
```

参考资料：

- [Error: failed to resolve output format · Discussion #2034 · wowchemy/wowchemy-hugo-themes (github.com)](https://github.com/wowchemy/wowchemy-hugo-themes/discussions/2034)
- [建站平台hugo-academic大升级 | Academic (huhuaping.com)](https://huhuaping.com/2020/10/05/hugo-big-update/)

解决方法，修改 `config/_default/config.yaml` 中的 outputs 内容为：

```yaml
outputs:
  # 默认内容为：
  #home: [HTML, RSS, JSON, WebAppManifest, headers, redirects]
  # 去掉 WebAppManifest
  home: [HTML, RSS, JSON, headers, redirects]
  section: [HTML, RSS]
```

然后执行:

```bash
hugo mod clean
hugo mod get -u ./...
hugo mod tidy
```

执行再执行 h 就可以成功了，和上面帖子中将 WebAppManifest 加回去还可以用不同，我如果把 WebAppManifest 加进去，只会继续抱同样的错。暂时先去掉 WebAppManifest吧。

备注：这个错误只在 m1 的 macbook 上遇到，intel macos 和 linux 下没这个问题。