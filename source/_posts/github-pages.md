title: "github pages"
date: 2015-06-15 12:54:37
tags: github blog hexo jekyll
---

# github 上搭建博客

现在很流行在github上建立静态blog。 诸如hexo 、jekyll等工具，可以很方便的实现在github上建立静态blog的功能，
但是这些工具如何在github上建立blog的？之前一直没有弄明白。(PS: 之前git小白，一直没有真正用github)
今天简单看了一下github pages的介绍， 按照教程简单操作了一遍，实现了在github上
搭建hello world的过程，这里把过程记录下来。
操作系统 Centos 6

### 准备
1， github账号
2， git工具

### 操作过程

1 登录[pages.github.com/](https://pages.github.com/  "github pages"), 
  点击"create a new repository"
![pages.github.com step1](http://7xij29.com1.z0.glb.clouddn.com/github_pagesgithub_pages_step1.png "pages.github.com step1" ) 


2 填写blog的入口地址yoursite.github.io
注意yoursite的名字要和你的github名字一致
![pages.github.com step2](http://7xij29.com1.z0.glb.clouddn.com/github_pagesgithub_pages_step1.png "pages github.com step2")  


3 在工作空间执行git clone命令，把新建的资源库clone到本地

![git clone 到本地](http://7xij29.com1.z0.glb.clouddn.com/github_pages_step3.png "git clone 资源库到本地" )


4 cd进入到资源库里面， 创建一个index.html

```
echo "Hello World" > index.html
```

5 提交到github

```
git add --all
git commit -m "hello world"
git push -u origin master

```

此时访问[mihenkii.github.io](http://mihenkii.github.io/) 如果看到了无比可亲的“Hello World"。 你就可以下一步了.

因为我们默认提交的是index.html， 所以不需要些文件名. 所以，此时可以联想到，hexo 、jekyll 应该是在index.html上做了些文章，给全站都加上了url路由.   下一步计划，搞起hexo.

 

如果提示如下403 错误， 按照如下步骤解决:

```
error: The requested URL returned error: 403 Forbidden while accessing https://github.com/mihenkii/mihenkii.github.io.git/info/refs
```

1  修改当前资源库下的.git/config文件如下：
![修改.git/config](http://7xij29.com1.z0.glb.clouddn.com/github_pages_step4.png "修改.git/config" )

2 此时如果还是提示失败，那么可能就是ssh证书有问题了. 可以尝试重做证书
```
ssh-keygen -t rsa -b 4096 -C "mihenkii@github.com"

```

然后把公钥添加到github上，再重试， 如果使用git的windows客户度，此时需要重启ssh-agent
![添加公钥到github](http://7xij29.com1.z0.glb.clouddn.com/github_pages_step5.png "添加公钥到github") 


关于详细的添加公钥到github的过程可以参考官网帮助连接
[help.github.com](https://help.github.com/articles/generating-ssh-keys/)



