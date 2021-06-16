title: docker_registry_delete_image
date: 2021-06-07 21:24:52
tags: "docker-delete-image"
---

#### 背景
自己搭建的docker registry， docker pull 报错
```
filesystem layer verification failed for digest sha256:xxx
```

#### 问题原因
应该是镜像的某一层layer校验不通过，参考:
https://github.com/distribution/distribution/issues/2168

#### 解决办法
删除镜像，重新上传一次镜像

###### 打开registry删除功能

进入到registry容器里，打开delete配置
```
docker container exec -it registry sh

vi /etc/docker/registry/config.yml

# 添加以下配置
storage:
  delete:
    enabled: true
```
重启容器

```
docker container restart registry
```

###### 删除镜像
这里以 zcloud/calico:v3.8.2 举例

```
# 查看镜像列表
curl -XGET 10.38.4.212:5000/v2/_catalog
# 查看镜像的tags
curl -XGET 10.38.4.212:5000/v2/zcloud/calico/tags/list
# 查看镜像详情
curl -XGET 10.38.4.212:5000/v2/zcloud/calico/manifests/v3.8.2

# 获取镜像digest
curl --header "Accept: application/vnd.docker.distribution.manifest.v2+json" -I -X GET 10.38.4.212:5000/v2/zcloud/calico/manifests/v3.8.2

# 删除镜像manifest
curl -I -X DELETE http://registry-internal.adp.aliyuncs.com:5000/v2/zcloud/calico/manifests/sha256:106a5b6fe7da462a562949c44d83a6327fea91bac93da1b61263113e7504963a

# 删除镜像文件
docker exec registry bin/registry garbage-collect /etc/docker/registry/config.yml
```

重启 
``` docker container restart registry ```

** 如果此时重新push同名镜像， 要等几分钟才才能pull到 **
