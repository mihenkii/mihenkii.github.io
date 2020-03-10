title: "bashrc vs bash_profile"
date: 2018-07-29 21:04:57
tags: "bashrc bash_profile"
---


#### 背景

> 使用Linux安装软件，有时候需要设置环境变量，有的文档建议设置在.bashrc里， 有的建议.bash_profile里
> 一般都能生效，那么区别是什么呢？

#### 介绍

> .bash_profile is executed for login shells, while .bashrc is executed for interactive non-login shells.

bash_profile: 登录试shell执行
bashrc: 非登录式shell执行

#### 什么是(非)登录试shell

登录试： 当你敲入用户名和密码登录的终端， 或通过ssh登录机器. .bash_profile会被调用
非登录: 当你已经登录了机器，在桌面环境打开终端， 或者在终端里面调用了/bin/bash, 此时.bashrc会被调用

#### 为什么会有这两个文件

比如，你希望第一次登录机器的时候，想看到机器的负载， 那么把uptime命令写到.bash_profile里面就可以了.
如果写到.bashrc里面， 你每次打开终端， 都会看到机器负载。

#### 推荐方法

当你设置PATH环境变量的时候， 把变量写到.bashrc
在.bash_profile里面加载.bashrc

```sh
if [ -f ~/.bashrc ]; then
   source ~/.bashrc
fi
```

#### 参考连接
[bash_profile_vs](http://www.joshstaiger.org/archives/2005/07/bash_profile_vs.html)
