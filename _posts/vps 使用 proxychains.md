---
title:   vps 使用 Proxychains
date: 2020-09-13 10:25:54
tags: 
 - proxychains
categories: 
 - linux
---
# 阿里云 Vps 使用 Proxychains

## 问题

> 阿里云 `vps` 无法连接 `github` ，可以使用梯子代理 `socks5`，用 `proxychains` 让 `vps` 走代理的端口

## v2ray

### 安装



> 自己的梯子是 `vmess` 协议的，其他协议另外百度

安装脚本

>curl -O https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh

> bash install-release.sh


> 如果 连接不通，可以在浏览器访问，保存脚本到 vps 执行

```shell
systemctl start v2ray   # 启动v2ray

systemctl enable v2ray  # 开机自启v2ray
systemctl status v2ray  # 查看v2ray状态
systemctl restart v2ray # 重启v2ry
systemctl stop v2ray    # 停止v2ray
systemctl disable v2ray # 移除v2ray开机自启
```

### 配置

> 配置文件位置：`/usr/local/etc/v2ray/config.json`

> 上面的安装脚本不会初始化配置，需要自己添加，我是直接将 win 下的客户端配置复制过去

```json
{
  "policy": null,
  "log": {
    "access": "",
    "error": "",
    "loglevel": "warning"
  },
  "inbounds": [
    {
      "tag": "proxy",
      "port": 10808,
      "listen": "127.0.0.1",
      "protocol": "socks",
      "sniffing": {
        "enabled": true,
        "destOverride": [
          "http",
          "tls"
        ]
      },
      "settings": {
        "auth": "noauth",
        "udp": true,
        "ip": null,
        "address": null,
        "clients": null
      },
      "streamSettings": null
    }
  ],
  "outbounds": [
    {
      "tag": "proxy",
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "主要配置",
            "port": 88,
            "users": [
              {
                "id": "主要配置",
                "alterId": 3,
                "email": "主要配置",
                "security": "auto"
              }
            ]
          }
        ],
        "servers": null,
        "response": null
      },
      "streamSettings": {
        "network": "ws",
        "security": null,
        "tlsSettings": null,
        "tcpSettings": null,
        "kcpSettings": null,
        "wsSettings": {
          "connectionReuse": true,
          "path": "/v2ray1",
          "headers": {
            "Host": "jqhkf.renyim.pw"
          }
        },
        "httpSettings": null,
        "quicSettings": null
      },
      "mux": {
        "enabled": true,
        "concurrency": 8
      }
    },
    {
      "tag": "direct",
      "protocol": "freedom",
      "settings": {
        "vnext": null,
        "servers": null,
        "response": null
      },
      "streamSettings": null,
      "mux": null
    },
    {
      "tag": "block",
      "protocol": "blackhole",
      "settings": {
        "vnext": null,
        "servers": null,
        "response": {
          "type": "http"
        }
      },
      "streamSettings": null,
      "mux": null
    }
  ],
  "stats": null,
  "api": null,
  "dns": null,
  "routing": {
    "domainStrategy": "IPIfNonMatch",
    "rules": [
      {
        "type": "field",
        "port": null,
        "inboundTag": [
          "api"
        ],
        "outboundTag": "api",
        "ip": null,
        "domain": null
      }
    ]
  }
}
```



## proxychains

### 安装



```shell
git clone https://github.com/rofl0r/proxychains-ng.git /usr/local/proxychains-ng
cd /usr/local/proxychains-ng
./configure --prefix=/usr --sysconfdir=/etc && make && make install && make install-config
```

### 配置

> 配置文件： `/etc/proxychains.conf`

> 将最下边的配置改为自己 `v2ray` 配置文件里的 ：`socks5  127.0.0.1 10808`

## 测试

> `wget` / `curl` 命令前加上 `proxychains4`