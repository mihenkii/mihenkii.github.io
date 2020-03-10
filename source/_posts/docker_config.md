title: "Docker配置"
date: 2017-11-13 22:04:57
tags: "docker centos"
---


#### 准备

> 配置docker registry 加速docker

#### 配置

```sh 
sudo vim /etc/docker/daemon.json

{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}

# 重启docker
sudo systemctl restart docker
# 查看镜像是否加载，我本机没有加载成功
sudo docker info

```

#### 拉取镜像

``` sh

docker pull ubuntu:14.04
docker image ls # 查看本地镜像

```

#### 启动实例

```
docker run ubuntu:14.04 # 如果本地没有这个image， 会从镜像仓库拉取并启动
docker run -it ubuntu:14.04 /bin/bash  # 启动并进入到实例

```

#### 退出实例
```
输入exit 或者ctrl+d 都会退出实例
如果想退出且不停止实例， 使用ctrl+p+q组合键退出
```

#### 登录实例

```
docker [container] exec -it $containerId /bin/bash # 新启动一个bash
docker [container] attach $containerId  #登录到旧的tty
```

#### 制作镜像 
新建一个目录，创建 Dockerfile

docker build -t 镜像名 .
docker run -p4000:80 镜像名
docker tag 镜像名 用户名/repository:tag
docker login # 输入用户名不带邮箱后缀

docker push 用户名/repository:tag
