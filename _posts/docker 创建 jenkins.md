---
title: docker 创建 jenkins
date: 2020-09-15 10:25:54
tags: 
 - docker
categories: 
 - docker
 - jenkins
---
# docker 创建 jenkins

## docker run

> 直接 `run`，默认 `latest` 版本

```
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v /usr/local/jenkins:/var/jenkins_home -u root jenkins
```

> -v ：配置文件持久化到本地
>
> -u：指定用户，否则会报权限不够 

## 修改配置文件

```shell
sed -i 's/www.google.com/www.baidu.com/g' /usr/local/jenkins/updates/default.json

sed -i 's/updates.jenkins-ci.org\/download/mirrors.tuna.tsinghua.edu.cn\/jenkins/g' /usr/local/jenkins/updates/default.json
```

## 其他问题

### 创建用户后跳转失败

> 去掉 `url` 中的 `jenkins`