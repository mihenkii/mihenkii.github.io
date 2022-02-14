title: centos-nginx-https-letsencrypt
date: 2022-02-14 14:26:01
tags: nginx https letsencrypt
---

## 介绍

早在2014年，Google就鼓励网站使用HTTPS协议，并优先考虑使用HTTPS的网站，从2017年开始，Google Chrome浏览器将非HTTPS的网站标记为不安全。
[Let's Encrypt](https://letsencrypt.org/zh-cn/getting-started/) 面向公众提供免费的HTTPS证书, 这里记录Nginx 开启https的过程。

## 注意点

我最初用Docker运行Tengine，希望将证书申请下来后mount到Docker里，目标是没有问题，但是操作起来过于麻烦，而且没成功，浪费了很多时间。
后来改用服务器直接跑Nginx， 并使用最基础的nginx.conf，一切都变得非常容易。

***尝试新事物的时候，最好是先“跑”起来，然后再研究更优的办法。***

1. 申请HTTPS证书的域名需要可以被公网访问
2. 如果使用Nginx，需要支持ssl, 编译的时候添加```--with-http_ssl_module```

## 操作步骤

#### 安装Certbot

服务器需要使用ACME协议从Let's Encrypt获取证书，我们使用官方推荐的 Certbot ACME客户端。

```
yum -y install epel-release
yum -y install certbot-nginx
```

#### 安装Nginx

我的服务器无法通过yum安装Nginx，只好自己编译一个。

下载Tengine(Nginx) 源码, 编译

```
./configure --prefix=/usr/local/nginx \
      --conf-path=/usr/local/nginx/conf/nginx.conf \
      --sbin-path=/usr/local/nginx/sbin/nginx \
      --pid-path=/var/run/nginx.pid \
      --lock-path=/var/run/lock/nginx.lock \
      --user=www \
      --group=www \
      --http-log-path=/var/log/nginx/access.log \
      --error-log-path=/var/log/nginx/error.log \
      --http-client-body-temp-path=/var/lib/nginx/client-body \
      --http-proxy-temp-path=/var/lib/nginx/proxy \
      --http-fastcgi-temp-path=/var/lib/nginx/fastcgi \
      --http-scgi-temp-path=/var/lib/nginx/scgi \
      --http-uwsgi-temp-path=/var/lib/nginx/uwsgi \
      --with-pcre-jit \
      --with-http_dav_module \
      --with-http_geoip_module \
      --with-http_gunzip_module \
      --with-http_gzip_static_module \
      --with-http_random_index_module \
      --with-http_realip_module \
      --with-http_stub_status_module \
      --with-http_secure_link_module \
      --with-http_ssl_module \
      --with-stream \
      --with-stream_ssl_module \
      --with-http_v2_module \
      --with-http_stub_status_module \
      --with-http_addition_module \
      --with-http_degradation_module \
      --with-http_flv_module \
      --with-http_mp4_module \
      --with-http_sub_module \
      --with-file-aio \
      --with-mail \
      --with-mail_ssl_module \
      --with-jemalloc

make && make install

# certbot会在PATH下找nginx，这里通过软连把nginx放到PATH下
ln -s /usr/local/nginx/sbin/nginx /usr/local/bin/nginx
ln -s /usr/local/nginx/conf /etc/nginx
```

增加www用户

```
useradd -s /sbin/nologin www
```

#### 配置Nginx

在网上随便找了个nginx.conf, 通过最低下的```include /etc/nginx/conf.d/*.conf;``` 加载个人的配置段。

```
user  www;
# This number should be, at maximum, the number of CPU cores on your system.
worker_processes auto;

error_log  /var/log/nginx/error.log error;
pid        /var/run/nginx.pid;


events {
    # The effective method, used on Linux 2.6+, optmized to serve many clients with each thread.
    use epoll;
    # Determines how many clients will be served by each worker process.
    worker_connections 4000;
    # Accept as many connections as possible, after nginx gets notification about a new connection.
    multi_accept on;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    # Allow the server to close the connection after a client stops responding.
    reset_timedout_connection on;
    client_header_timeout 15;
    # Send the client a "request timed out" if the body is not loaded by this time.
    client_body_timeout 10;
    # If the client stops reading data, free up the stale client connection after this much time.
    send_timeout 15;
    # Timeout for keep-alive connections. Server will close connections after this time.
    keepalive_timeout 30;
    # Number of requests a client can make over the keep-alive connection.
    keepalive_requests 30;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';


    client_body_buffer_size 128k;
    client_max_body_size 10m;
    proxy_read_timeout 180s;

    # Compression.
    gzip on;
    gzip_min_length 10240;
    gzip_proxied expired no-cache no-store private auth;
    gzip_types text/plain text/css text/xml text/javascript application/x-javascript application/xml;
    gzip_disable "msie6";

    # Sendfile copies data between one FD and other from within the kernel.
    sendfile on;
    # Don't buffer data-sends (disable Nagle algorithm).
    tcp_nodelay on;
    # Causes nginx to attempt to send its HTTP response head in one packet,  instead of using partial frames.
    tcp_nopush on;


    # Hide web server information
    server_tokens off;
    server_info off;
    server_tag off;

    # redirect server error pages to the static page
    error_page 404             /404.html;
    error_page 500 502 503 504 /50x.html;

    include /etc/nginx/conf.d/*.conf;
}
```

#### 配置vhost

下面是我的vhost配置文件, 使用最基础的配置文件
# cikii.com.conf
```
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name     cikii.com www.cikii.com;
    root /usr/local/nginx/html/;
}

```

# jiu.cikii.com.conf
```
server {
    listen 80;
    listen [::]:80;
    server_name     jiu.cikii.com;
    root /usr/local/nginx/html/;
}

```

重启一次Nginx


#### 申请证书

给3个域名同时申请证书, 3个证书会写在同一个pem文件里

```
certbot --nginx -d cikii.com -d www.cikii.com -d jiu.cikii.com
```

如果没有报错的话， 我们自己的vhost配置文件已经被certbot改成支持HTTPS的配置了。

最后, 记得在crontab里定时renew证书

```
0 12 * * * /usr/bin/certbot renew --quiet
```

## 参考文档

https://www.nginx.com/blog/using-free-ssltls-certificates-from-lets-encrypt-with-nginx/
