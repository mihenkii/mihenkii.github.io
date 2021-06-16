title: docker-commmon-error
date: 2021-06-16 10:50:32
tags:
---

#### 1. docker registiry pull Error http: server gave HTTP response to HTTPS client
```
docker pull 192.168.0.10:5000/centos
Error response from daemon: Get https://192.168.0.10:5000/v2/: http: server gave HTTP response to HTTPS client
```
解决办法: 在daemon.json里面添加insecure-registries
```
# Mac ~/.docker/daemon.json 
# Centos: /etc/docker/daemon.json 

{"experimental":false,"debug":true,"insecure-registries":["192.168.0.10:5000"]}
```
重启docker
