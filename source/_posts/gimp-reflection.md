title: "gimp 倒影效果"
date: 2015-09-27 11:24:57
tags: "gimp"
---

## gimp 实现倒影效果

最终效果如下：

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

