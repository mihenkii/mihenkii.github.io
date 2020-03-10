title: "Linux su nopassword"
date: 2017-07-15 11:24:57
tags: "linux"
---

#### Linux 普通用户su到另一个普通用户

> 工作中很少使用root用户，存在使用个人用户，切换到work账户的场景
> 比如我的个人账户是mihenkii， 要切换的目标账户是work

#### 实现

``` sh

sudo vim /etc/pam.d/su

auth       [success=ignore default=1] pam_succeed_if.so user = work
auth       sufficient   pam_succeed_if.so use_uid user = mihenkii

# 解释:  第一行是确定目标用户是work，如果是work，且调用的用户是mihenkii，那么第二行mihenkii会成功授权切换到work

```

#### 参考连接

[stackexchange](https://unix.stackexchange.com/questions/113754/allow-user1-to-su-user2-without-password)
