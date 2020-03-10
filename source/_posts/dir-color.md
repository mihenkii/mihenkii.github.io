title: "Linux 目录颜色"
date: 2017-06-11 11:24:57
tags: "linux"
---

#### Linux 终端ls目录带有颜色

> 新装的Linux系统， 执行ls的时候默认没有颜色，不够美观

#### 实现

ls增加 —color=auto 参数

``` sh
echo "alias ls='ls --color=auto'" >> ~/.bashrc
```

—color=auto 这种方式会使部分文件显示带有色彩，但是仍然不够

``` sh
echo "LS_COLORS=$LS_COLORS:'fi=00:di=00;34:ln=00;36:pi=40;33:so=00;35:bd=40;33;01:cd=40;33;01:or=01;05;37;41:mi=01;05;37;41:ex=00;32:*.exe=00;32:*.sh=00;32:*.csh=00;32:*.tar=00;31:*.tgz=00;31:*.taz=00;31:*.lzh=00;31:*.zip=00;31:*.z=00;31:*.Z=00;31:*.gz=00;31:*.bz2=00;31:*.bz=00;31:*.tz=00;31:*.rpm=00;31:*.jpg=00;35:*.gif=00;35:*.bmp=00;35:*.xbm=00;35:*.xpm=00;35:*.png=00;35:*.tif=00;35:*.md=00;32:'" >> ~/.bashrc && source ~/.bashrc

```

#### 说明

di=00;34:
[文件类型]=[字体];[颜色]:
详细可选参数列表
di = directory
fi = file
ln = symbolic link
pi = fifo file
so = socket file
bd = block (buffered) special file
cd = character (unbuffered) special file
or = symbolic link pointing to a non-existent file (orphan)
mi = non-existent file pointed to by a symbolic link (visible when you type ls -l)
ex = file which is executable (ie. has ‘x’ set in permissions).
0 = default colour
1 = bold
4 = underlined
5 = flashing text
7 = reverse field
31 = red
32 = green
33 = orange
34 = blue
35 = purple
36 = cyan
37 = grey
40 = black background
41 = red background
42 = green background
43 = orange background
44 = blue background
45 = purple background
46 = cyan background
47 = grey background
90 = dark grey
91 = light red
92 = light green
93 = yellow
94 = light blue
95 = light purple
96 = turquoise
100 = dark grey background
101 = light red background
102 = light green background
103 = yellow background
104 = light blue background
105 = light purple background
106 = turquoise background

#### 参考连接

[askubuntu](https://askubuntu.com/questions/466198/how-do-i-change-the-color-for-directories-with-ls-in-the-console)

![gimp文字倒影](http://7xij29.com1.z0.glb.clouddn.com/gimp_reflect_0.png)

本文用了相对简单易懂的方式实现了文字倒影。
还有一种方式是在“倒影”图像是用“橡皮”擦出倒影效果，这里不推荐。

## Step by step

1 Ctrl + n  新建一个空白图形，设置合适的大小

![gimp 新建图像](http://7xij29.com1.z0.glb.clouddn.com/gimp_reflect_1.png)

2 选择文字工具， 写下自己想要设计的文字，并调整大小，个人喜欢用75的大小

![gimp文字工具](http://7xij29.com1.z0.glb.clouddn.com/gimp_reflect_2.png)

3 选中文字，设置文字颜色，并记住文字颜色(这个颜色后面会用到)

![gimp设置文字颜色](http://7xij29.com1.z0.glb.clouddn.com/gimp_reflect_3.png)

4 复制图层，用来做"倒影"

![复制图层](http://7xij29.com1.z0.glb.clouddn.com/gimp_reflect_4.png)

5 移动复制的图层到原图的正下方

![gimp移动工具](http://7xij29.com1.z0.glb.clouddn.com/gimp_reflect_5.png)

6 翻转图形， 并在此调整文字位置，移动翻转后的图形到原图的正下方

![gimp翻转图形](http://7xij29.com1.z0.glb.clouddn.com/gimp_reflect_6.png)

7 制作倒影, 如图：首先在倒影的图层上点击"锁定Alpha通道", 然后点击"左侧黑色", 设置颜色和文字一致。

![gimp混合工具](http://7xij29.com1.z0.glb.clouddn.com/gimp_reflect_7.png)

8 图像合并

![合并图像](http://7xij29.com1.z0.glb.clouddn.com/gimp_reflect_8.png)


参考连接：
[gimp-tutorials.net](http://gimp-tutorials.net/node/236)

[gimp-tutorials.net](http://gimp-tutorials.net/node/91)

