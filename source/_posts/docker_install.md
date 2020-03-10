title: "Docker环境搭建"
date: 2017-11-13 21:04:57
tags: "docker centos"
---


#### 准备

> Centos : Centos7 64位
> docker-ce: Docker-ce-17

#### 安装

1.卸载本地原有docker

```sh 
sudo yum remove docker-common docker-selinux docker-engine
```

2.安装docker依赖软件和yum源

``` sh
sudo yum install yum-utils
sudo yum install device-mapper-persistent-data
sodu yum instlal lvm2
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce
```

3.启动docker

``` sh
sudo systemctl start docker
```

4.验证docker

``` sh
sudo docker run hello-world
```

5.添加运行docker账号
> docker的daemno会启动一个socket，只有root和docker组用户可以访问， 普通账号无法访问
> 所以需要把普通用户添加到docker组才可以访问

```sh
sudo groupadd docker
sudo usermod -aG docker work
```

6.设置开启启动

``` sh 
sudo systemctl enable docker
```

[参考文档](https://docs.docker.com)
