---
title: "外观和体验"
date: 2021-08-13
weight: 220
description: >
  docsy的外观和体验
---

> 参考： [Look and Feel | Docsy](https://www.docsy.dev/docs/adding-content/lookandfeel/)

### 颜色和css

这些暂时先用默认吧。

### 代码高亮

继续用默认的 Chroma 和 tango style。

### Mermaid的支持

有 plantuml 就先不用这个。

### plantuml的支持

试试这个：

```plantuml
participant participant as Foo
actor       actor       as Foo1
boundary    boundary    as Foo2
control     control     as Foo3
entity      entity      as Foo4
database    database    as Foo5
collections collections as Foo6
queue       queue       as Foo7
Foo -> Foo1 : To actor 
Foo -> Foo2 : To boundary
Foo -> Foo3 : To control
Foo -> Foo4 : To entity
Foo -> Foo5 : To database
Foo -> Foo6 : To collections
Foo -> Foo7: To queue
```

开启方式：

```properties
[params.plantuml]
enable = true
theme = "default"

#Set url to plantuml server 
#default is http://www.plantuml.com/plantuml/svg/
svg_image_url = "https://www.plantuml.com/plantuml/svg/"
```

注意 guessSyntax 必须设置为false，否则无法生效：

```toml
[markup]
  [markup.highlight]
      guessSyntax = "false"
```