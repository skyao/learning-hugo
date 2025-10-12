---
title: "增加更多语言的语法高亮"
linkTitle: "增加高亮"
date: 2021-11-08
weight: 250
description: >
  默认支持的编程语言有限，可以在需要时增加语言的支持，使之也可以做到语法高亮
---

## 默认支持的语言列表

hugo 采用 Prism 进行编程语言的语法高亮，由于编程语言数量太多，通常也不需要支持所有的语言（会导致js和css文件过大），因此默认只支持以下语言：

- markup
- css
- clike
- javascript
- bash
- c
- csharp
- cpp
- go
- java
- markdown
- python
- scss
- sql
- toml
- yaml

其他语言是无法支持的，比如默认 rust 语言就不被支持。如果需要增加对rust语言的支持，可以进行定制。

## 定制新的语言支持列表

步骤如下：

1. 打开任意页面，查看源代码，找到 `<script src='/js/prism.js'></script>` 这段，通常在页面最下方

2. 打开这个 js 文件，比如 http://localhost:1313/js/prism.js ，可以看到最前面有这么一排注释：

    ```
    /* PrismJS 1.25.0
    https://prismjs.com/download.html#themes=prism&languages=markup+css+clike+javascript+bash+c+csharp+cpp+go+java+markdown+python+rust+scss+sql+toml+yaml&plugins=toolbar+copy-to-clipboard */
    ```
    
    这里有版本和支持语言列表。

3. 打开上面的 URL 地址，在该页面增加需要支持的语言，如:

	- HTTP
	- Lua
	- Makefile
	- Rust 
	- .properties
	- Protocol Buffers
	- WebAssembly

	也可以添加插件
	
	- Line Highlight
	- Line Numbers
	- Highlight Keywords

4. 点击页面下方的“download js”和“download css”，下载 prism.js 文件和 prism.css 文件

    此时 js 文件前面的内容变为：

    ```
    /* PrismJS 1.25.0
    https://prismjs.com/download.html#themes=prism&languages=markup+css+clike+javascript+bash+c+csharp+cpp+go+java+lua+makefile+markdown+properties+protobuf+python+rust+scss+sql+toml+wasm+yaml&plugins=line-highlight+line-numbers+highlight-keywords+toolbar+copy-to-clipboard */
    ```

5. 将上述文件分别复制到hugo项目根目录下，路径为： `static/css/prism.css` 和 `static/js/prism.js` ，这会覆盖默认的文件。


## 其他：高亮的参数设置

可以高亮某些行，现实代码行数等。

https://gohugo.io/content-management/syntax-highlighting/

