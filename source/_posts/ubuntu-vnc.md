title: Ubuntu-VNC
date: 2021-07-15 10:21:05
tags: Ubuntu20.04 VNC
---
## 背景
需要使用Easy Connect链接到某单位的VPN，公司电脑不允许安装VPN客户端，因此考虑在阿里云Ubuntu Server上安装VNC，本地Mac通过VNC链接到Ubuntu

## 安装步骤

我这里直接使用root账号进行的操作，普通账号命令前面加上sudo

#### 安装桌面环境
```
apt update
apt install xfce4 xfce4-goodies

# xdm 可能不需要安装
apt install xdm
```

#### 安装tightvncserver

```
apt install tightvncserver
# xrdp 也是一个远程桌面服务，支持usb，音频
apt install xrdp
```

#### 启动vncserver
```
vncserver
```
此时会提示设置密码, 设置一个自己熟悉的密码，如果需要更改密码，可以使用vncpasswd命令

## 配置vnc
需要配置VNC链接到哪个桌面环境
首先停掉当前的VNC实例
```
vncserver -kill :1
```
备份原始配置
```
mv ~/.vnc/xstartup ~/.vnc/xstartup.bak
```
编辑配置如下
```
# cat ~/.vnc/xstartup
#!/bin/sh
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
startxfce4 &
```

**如果没有unset那两个变量，通过VNC链接到Ubuntu是灰屏**

## 设置vnc安全链接

vnc默认没有使用加密通讯，需要使用SSH tunnel来建立安全链接

在你的Mac本地执行如下命令, Windows用户见“参考链接”里面的putty配置
```
# 这条命令需要一直运行
ssh -L 59000:localhost:5901 -C -N -l root 47.199.13.30
```

-L: 参数代表将我本地Mac的59000端口转发到目标机器(47.199.13.30)的IP：port上. 可以理解登录到目标机器以后，在目标机器上执行 localhost:5901
-C: 参数代表启用压缩传输
-N: 代表ssh只做端口转发，不会执行命令
-l: 代表login user

## 链接VNC
在Mac上打开Finder，点击Go，选择Connect to Server
链接到本机的59000端口
```
vnc://localhost:59000
```
## 问题

1. VNC安装成功以后，EasyConnect还是没有搞定，可能与xfce桌面环境有关系，建议使用Gnome 
2. Ubuntu默认的浏览器无法使用，提供input/output error
```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
apt install ./google-chrome-stable_current_amd64.deb

# 在设置里，找到Preferred Applications  -> Web Browser -> Other... 输入/usr/bin/google-chrome
```
3. 浏览器显示中文乱码
安装中文字库
```
apt-get install ttf-wqy-zenhei
apt-get install language-pack-zh-hans
```


## 参考链接

https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-20-04
https://cat.pdx.edu/platforms/mac/remote-access/vnc-to-linux/
https://help.ubuntu.com/community/VNC/Servers

