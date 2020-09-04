---
title: demo
date: 2020-09-04 10:25:54
tags: 
 - https
categories: 
 - nginx
---
# nginx 设置 https

## 准备工作

>  安装 git、nginx

## 步骤

### 下载 let encrypt



```
cd /usr/local
git clone https://github.com/certbot/certbot
cd cerbot
systemctl stop nginx 
./letsencrypt-auto certonly --standalone --email zuishuaiweiwei@gmail.com -d zuishuaiweiwei.com -d www.zuishuaiweiwei.com
```

![image-20200904130118361](https://cdn.jsdelivr.net/gh/zuishuaiweiwei/jcdn/img/20200904130118.png)

提示这个表示成功

### 设置 nginx

> 修改内容

```conf
server {
listen       443 ssl;
server_name  www.zuishuaiweiwei.com zuishuaiweiwei.com;
ssl_certificate /etc/letsencrypt/live/zuishuaiweiwei.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/zuishuaiweiwei.com/privkey.pem;
ssl_session_timeout 5m;
ssl_protocols SSLv3 TLSv1;
ssl_ciphers ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv3:+EXP;
ssl_prefer_server_ciphers on;

location / {
root /usr/local/hexo-wei/public;
index index.html;
	}
}

server {
listen       80;
server_name  www.zuishuaiweiwei.com zuishuaiweiwei.com;
return 301 https://$server_name$request_uri;
}
```

> 重启 nginx



