---
title: "百度统计"
date: 2021-08-19
weight: 337
description: >
  启用百度统计来跟踪网站访问情况
---

没有找到 docsy 主题支持百度统计的方式，只好暴力添加。

修改 docsy 主题中的 `layouts/partials/footer.html` 文件，在 `<footer>` 标签中强制加入一下内容：

```html
<script>
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "https://hm.baidu.com/hm.js?17048c9d4209e5d08a9ae5b0b4160808";
  var s = document.getElementsByTagName("script")[0]; 
  s.parentNode.insertBefore(hm, s);
})();
</script>
```

不是一个好办法，但好歹能工作。

