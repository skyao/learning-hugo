---
title: "开启HTTPS"
date: 2021-01-18
weight: 230
description: >
  如何为Hugo增加HTTPS支持
---

参考文章：

- [给博客加上HTTPS](https://blog.qikqiak.com/post/make-https-blog/)
- [在Ubuntu上获取Let’s Encrypt免费证书](https://tsukkomi.org/post/get-the-lets-encrypt-certificate-on-ubuntu)

由于我用的博客服务器是ubuntu 16.04，因此部分命令稍有不同。

## 生成证书

先安装工具：

```bash
sudo apt-get install letsencrypt
```

生成证书：

```bash
sudo letsencrypt certonly --webroot -w /var/www/skyao -d skyao.io
```

## 配置nginx

在`/etc/nginx/sites-available`下增加一个`skyao.io.https`站点文件，内容如下：

```json
server {
    listen 443 ssl;
    server_name skyao.io www.skyao.io;

    root /var/www/skyao;
    index index.html;

    ssl on;
    ssl_certificate /etc/letsencrypt/live/skyao.io/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/skyao.io/privkey.pem;
}
```

然后将http请求都自动转为https，修改原来的`skyao.io`配置文件：

```json
server {
    listen 80;
    server_name skyao.io www.skyao.io;
    rewrite ^(.*)$  https://$host$1 permanent;
}
```

重启nginx：

```bash
sudo service nginx restart
```

## 设置自动更新证书

由于Let's Encrypt证书的有效期为90天，所有我们需要定期更新以避免证书过期，通常Let's Encrypt会发邮件提醒的。

更新操作如下：

```bash
# 更新证书
sudo letsencrypt renew
# 重新启动nginx
sudo systemctl restart nginx
```

