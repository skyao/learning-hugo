---
title: "启用CSE搜索"
date: 2021-01-20
weight: 335
description: >
  使用google CSE来搜索站内内容
---

### 创建cse搜索

1. 登录cse网站

   https://cse.google.com/cse/all
   
   可以看到当前已经添加的搜索引擎
   
2. 添加一个搜索引擎

   注意在"外观" -> "布局" 中设置 "由google托管"，然后"搜索结果会现在以下位置"中选择"当前窗口"

3. 获取 **搜索引擎 ID** 之后，修改 config.toml :

   ```toml
   gcs_engine_id = "8f763efbce07435e9"
   #gcs_engine_id = "011737558837375720776:fsdu1nryfng"
   ```

4. 在页面上尝试搜索





