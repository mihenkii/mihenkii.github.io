title: "win7上安装centos7双系统"
date: 2017-06-11 11:04:57
tags: "win7 centos7"
---


> 09年的笔记本电脑，CPU: 奔腾T4200不支持vt-x, 无法在Virtual Box中安装64位Centos系统

#### 准备

1. 8G U盘
2. UltraISO 用来制作启动盘
3. CentOS.iso
4. 分区助手(其实没有真正用到)

#### 操作过程

1. 清理一块硬盘分区来安装系统
1.1 [计算机]-右键[管理]-[磁盘管理]-[删除卷]. 这个删除的卷，就是准备安装Centos的位置
2. 制作U盘系统盘
2.1 将CentOS镜像下载到本地, 我选择的是：CentOS-7-x86_64-DVD-1611.iso
2.2 将U盘插到电脑，打开UltraISO, 打开下载的ISO文件, 然后选择[启动] - [写入硬盘映像] 写入方式选择[USB-HDD+]
2.3 等待10分钟左右系统盘制作完成
3. 安装系统
3.1 开启按F2, 进入Bios页面, boot选择从usb启动, 如果运气好，就可以直接安装系统

#### 遇到问题

1. centos7 failed to start switch root

安装系统遇到上面的错误很常见, 网上给出的解决办法：
将 vmlinuz initrd=initrd.img inst.stage2=hd:LABEL=CentOS\x207\x20x86_64 quiet
改为 vmlinuz initrd=initrd.img inst.stage2=hd:/dev/sdb4 quiet

> /dev/sdb4代表你的U盘(有的文章说是准备安装系统的盘，描述不准确)
Note 引用RedHat官网介绍
By default, the inst.stage2= boot option is used on the installation media and set to a specific label (for example, inst.stage2=hd:LABEL=RHEL7\x20Server.x86_64). If you modify the default label of the file system containing the runtime image, or if using a customized procedure to boot the installation system, you must ensure this option is set to the correct value.

如果不确定U盘到底是/dev/sdxx, 可以用另一种解决方法：
inst.stage2=hd:LABEL=分区卷标(注意LABEL这单词不能删除掉)
通过上面提到的分区助手可以修改[分区卷标], 打开软件，在U盘上[右键]-[设置卷标]即可

我用的命令是：

```
vmlinuz initrd=initrd.img inst.stage2=hd:LABEL=CentOS7 quiet   # 区分大小写

```

#### 设置grub2

在centos的/boot/grub2/grub.cfg

``` sh
menuentry "Windows 7" {   set root='(hd0,1)'
   chainloader +1}
```

#### 参考链接

[http://www.osyunwei.com/archives/7829.html](http://www.osyunwei.com/archives/7829.html)
[http://blog.csdn.net/txl199106/article/details/41344399](http://blog.csdn.net/txl199106/article/details/41344399)
[redhat](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-anaconda-boot-options.html#list-boot-options-sources)

