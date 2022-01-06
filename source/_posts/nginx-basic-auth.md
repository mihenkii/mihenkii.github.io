title: nginx-basic-auth
date: 2022-01-06 13:56:39
tags: nginx
---

> 公司的安全团队扫描到我的系统后台直接暴露在公网，有安全问题，需要加上Basic Auth鉴权。

#### 前置要求

需要安装httpd-tools（Centos）, Debian系列需要安装apache2-utils

```
yum install -u httpd-tools
```

#### 操作步骤

1. 创建htpasswd文件

```
htpasswd -c /usr/local/nginx/conf/htpasswd admin
```

2. 在nginx.conf里配置鉴权

```
server {
    ...
    auth_basic  "Administrator's Area";
    auth_basic_user_file /usr/local/nginx/conf/htpasswd;

    # 不需要开启鉴权的path设置成auth_basic off
    location /test/ {
        auth_basic off;
    }
}
```

#### 参考文档

https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/
