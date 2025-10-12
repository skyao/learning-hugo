---
title: "搭建CloudRuntime官方网站"
linkTitle: "CloudRuntime网站"
weight: 110
description: >
  修改模版内容，搭建CloudRuntime网站
---

准备修改 docsy-example 的内容，搭建CloudRuntime官方网站。

## 准备仓库

创建 `cloudruntime/website` 仓库，使用 main 分支：

```bash
cd cloudruntime
git clone git@github.com:cloudruntime/website.git
cd website
git remote add upstream git@github.com:cloudruntime/docsy-example.git
git fetch --all
git merge upstream/cloudruntime --allow-unrelated-histories
```

修改 `README.md` 中的冲突内容，然后一起提交。

## 修改用户案例

文档需要单独作为独立的 hugo 项目存在，cloudruntime 的 website 仓库不存放这个内容，暂时修改用户案例，后续有用户使用之后可以把这块建设起来。

将 docs 目录重命名为 case，修改 _index.md 文件，增加以下内容：

```yaml
cascade:
- type: "docs"
```



## 增加导航条目

主要是增加 使用文档 和 开发文档 两个顶级导航条目，分别指向 docs 和 devdocs 两个子目录。

修改 hugo.toml，增加如下内容，这样可以在增加导航条的同时实现 i18n：

```toml
[[languages.en.menu.main]]
name = "Documentation"
weight = 20
url = "https://cloudruntime.net/docs/"
[[languages.en.menu.main]]
name = "Development"
weight = 30
url = "https://cloudruntime.net/devdocs/"

[[languages.zh-cn.menu.main]]
name = "使用文档"
weight = 20
url = "https://cloudruntime.net/docs/"
[[languages.zh-cn.menu.main]]
name = "开发文档"
weight = 30
url = "https://cloudruntime.net/devdocs/"
```

有多种方式，具体参考：

https://gohugo.io/content-management/multilingual/#menus



## 删除示例内容

删除 blog 和 case （原docs）下的示例内容，将内容置空以便后续增加。



## 发布网站内容

### 准备工作

建立目录：

```bash
mkdir -p ~/site/cloudruntime
cd ~/site/cloudruntime
```

升级 npm

```bash
cd ~/temp
wget https://nodejs.org/dist/v20.10.0/node-v20.10.0-linux-x64.tar.xz
tar xvf node-v20.10.0-linux-x64.tar.xz
sudo rm -rf /usr/share/nodejs
sudo mv node-v20.10.0-linux-x64 /usr/share/nodejs
```

安装新版本的 hugo extension 版本：

```bash
cd ~/temp
wget https://github.com/gohugoio/hugo/releases/download/v0.121.1/hugo_extended_0.121.1_linux-amd64.tar.gz
tar xvf hugo_extended_0.121.1_linux-amd64.tar.gz
sudo cp hugo /usr/local/bin/hugo121
```

### 准备 docsy 主题

```bash
cd ~/site/cloudruntime
git clone https://github.com/cloudruntime/docsy.git
cd docsy
git checkout local-files

npm install --save-dev autoprefixer
npm install --save-dev postcss-cli
npm install -D postcss
```

### 准备 cloudruntime 的 发布目录

```bash
cd /var/www/
sudo mkdir cloudruntime
sudo chown sky cloudruntime 
sudo chgrp sky cloudruntime
```

### 发布 website 网站

```bash
cd ~/site/cloudruntime
git clone https://github.com/cloudruntime/website.git
cd website

./public.sh
```

### 配置 nginx

```bash
sudo vi /etc/nginx/sites-available/cloudruntime.net
```

内容如下：

```properties
server {
    listen 80;
    server_name cloudruntime.net www.cloudruntime.net;
    
    root /var/www/cloudruntime;
    index index.html;
    
    location / {
        try_files $uri $uri/ =404;
    }
}
```

增加 sites-enabled ：

```bash
sudo ln -s /etc/nginx/sites-available/cloudruntime.net /etc/nginx/sites-enabled/cloudruntime.net
```

重启 nginx：

```bash
sudo systemctl restart nginx
```

用浏览器访问地址 http://cloudruntime.net/ 进行验证。

### 开启 https

执行下面命令生成 ssl 证书：

```bash
sudo letsencrypt certonly --webroot -w /var/www/cloudruntime -d cloudruntime.net
```

增加 https 的 site：

```bash
sudo vi /etc/nginx/sites-available/cloudruntime.net.https
```

内容如下：

```properties
server {
    listen 443 ssl;
    server_name cloudruntime.net www.cloudruntime.net;

    root /var/www/cloudruntime;
    index index.html;

    ssl on;
    ssl_certificate /etc/letsencrypt/live/cloudruntime.net/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/cloudruntime.net/privkey.pem;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

增加 sites-enabled ：

```bash
sudo ln -s /etc/nginx/sites-available/cloudruntime.net.https /etc/nginx/sites-enabled/cloudruntime.net.https
```

重启 nginx：

```bash
sudo systemctl restart nginx
```

用浏览器访问地址 https://cloudruntime.net/ 进行验证。

然后将http请求都自动转为https，修改原来的`cloudruntime.net`配置文件：

``` bash
sudo vi /etc/nginx/sites-available/cloudruntime.net
```

内容修改为：

```properties
server {
    listen 80;

    server_name cloudruntime.net www.cloudruntime.net;
    rewrite ^(.*)$  https://$host$1 permanent;
}
```

用浏览器访问地址 http://cloudruntime.net/ 进行验证，会自动跳转到 https 的页面。
