title: k8s-nginx-pod-mount-ssl
date: 2021-10-27 19:55:02
tags: secert ssl
---
#### 0.背景
> 私有化部署的Tengine需要开启HTTPS，挂载自签名的证书

#### 1.生成证书
```
# --nodes 不需要密码加密
openssl req -x509 -nodes -days 36500 -newkey rsa:2048 -keyout tls.key -out tls.crt
```
生成证书的几个字段说明
```
Country Name (2 letter code) [AU]:US
State or Province Name (full name) [Some-State]:New York
Locality Name (eg, city) []:New York City
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Bouncy Castles, Inc.
Organizational Unit Name (eg, section) []:Ministry of Water Slides
Common Name (e.g. server FQDN or YOUR name) []:server_IP_address
Email Address []:admin@your_domain.com
```

#### 2.证书Base64编码
> The values for all keys in the data field have to be base64-encoded strings. If the conversion to base64 string is not desirable, you can choose to specify the stringData field instead, which accepts arbitrary strings as values.
```
cat tls.crt | base64 | xargs -n 100 | sed 's/ //g'
cat tls.key | base64 | xargs -n 100 | sed 's/ //g'
```

#### 3.创建Secret对象
```
apiVersion: v1
kind: Secret
metadata:
  name: tls-certs
  namespace: {{ .Release.Namespace }}
type: kubernetes.io/tls
data:
  tls.crt: "将第2步的crt字符串粘贴在这里"
  tls.key: "将第2步的key字符串粘贴在这里"
```
#### 4.定义Volume
```
      volumes:
      - name: certs
        secret:
          secretName: tls-certs
          defaultMode: 420
      - name: nginx-config
        configMap:
          name: tengine-config
          items:
          - key: nginx.conf
            path: nginx.conf
      - name: logdir
        emptyDir: {}
```
#### 5.定义volumeMount
```
        volumeMounts:
        - mountPath: /etc/nginx/certs
          name: certs
        - mountPath: /etc/nginx/nginx.conf
          name: nginx-config
          subPath: nginx.conf
```

#### 6.参考文档
https://kubernetes.io/docs/concepts/configuration/secret/
https://blog.csdn.net/ljx1528/article/details/108491764
https://www.cnblogs.com/micro-chen/p/11794248.html
https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-in-ubuntu-18-04
