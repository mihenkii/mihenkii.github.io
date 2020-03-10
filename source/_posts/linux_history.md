title: "linux history 添加时间"
date: 2018-07-28 21:04:57
tags: "linux history stamp"
---


#### 背景

> 在Linux上执行history命令默认不现实命令执行时间，对于问题排查不是很友好.
> 通过设置，可以在history命令前面加上命令执行时间

#### 安装

bash

```sh 
HISTTIMEFORMAT=%Y-%m-%d %H:%M:%S
HISTSIZE=1000 # 历史命令数量
```

zsh

``` sh
HIST_STAMPS="%Y-%m-%d %H:%M:%S"
HISTSIZE=50000
```

加载配置

``` sh
# bash
source ~/.bashrc

#zsh
source ~/.zshrc

```

#### 效果

```
history
1  2018-07-28 16:36:10  cd /opt/workspace
2  2018-07-28 16:36:20  vim linux_history.md
3  2018-07-28 16:36:33  env
4  2018-07-28 16:36:40  vim linux_history.md
5  2018-07-28 16:37:52  ls
6  2018-07-28 16:37:56  hexo server
7  2018-07-28 16:38:19  ls
8  2018-07-28 16:38:21  vim linux_history.md

```
