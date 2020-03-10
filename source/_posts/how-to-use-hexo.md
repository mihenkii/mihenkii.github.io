title: "使用hexo创建静态blog"
date: 2015-06-15 20:44:07
tags: hexo posts pages
---

# 使用hexo创建blog

上一篇 [github pages](http://www.mihenkii.tk/)， 说明了如何在github pages上挂一个自己的"Hello World"页面， 了解最基本的以后，接下来就尝试使用hexo这个工具来搭建静态博客了.

### 名词解释： 
静态博客 : 全部由静态页面组成的blog， 不需要和数据库交互的页面
hexo:  一个管理静态blog的工具
post:  发的一个文章，就是一个post
pages: 每一个单独的页面， 就是一个pages， 比如 个人介绍页面, 

![posts pages](http://7xij29.com1.z0.glb.clouddn.com/blog_hexo_step4.png)

首先，简单介绍一下hexo， 这个一个台湾同胞创建的工具， 可以方便的管理静态blog， 当然，最主要的功能，是这货支持Markdown.

### 系统要求：
git
node.js

hexo的安装非常简单，就一条命令：
```
$ npm install -g hexo-cli
```

### 具体操作
如果你是第一次搭建blog， 那么请理解下面的步骤：

##### 1， 创建工作空间(非必须)， 个人习惯， 我习惯把一些和代码相关的东西放到/opt/workspace
```
mkdir -pv /opt/workspace
cd /opt/workspace
```

##### 2,  初始化blog空间
```
hexo init blogtest

```
![初始化blog](http://7xij29.com1.z0.glb.clouddn.com/blog_hexo_step1.png)

##### 3,  使用hexo创建第一篇hellowold
```
# 进入到blog目录
cd blogtest
hexo new helloworld
```
如果看到下面的提示， 说明目前一切OK
![创建post](http://7xij29.com1.z0.glb.clouddn.com/blog_hexo_step2.png)

如果提示了hexo的usage， 请检查是否进入到了blog目录内，如果是，可能是hexo安装时没有加-g参数
执行下面命令，然后再重试
```
npm install hexo --save
```
##### 4, 编辑post.

``` sh
vim source/_posts/helloworld.md
```
![编辑文件，写下Hello World](http://7xij29.com1.z0.glb.clouddn.com/blog_hexo_step3.png)5

##### 5, 下面是生成阶段， 也就是把你写的markdown文件转化成html文件.
```
hexo generate 
```
##### 6,  本地预览
```
# 会在本地建立一个server， 端口一般是4000， 可以在浏览器访问看效果
hexo server
```

##### 7, 生成完以后提交到github pages就完成了
```
hexo deploy
```

此时，blog就被put到github pages上了.
如果不是在图形界面执行，此步骤可能出现git提交失败的问题，可以参考[上篇](http://www.mihenkii.tk/)

更多关于hexo的命令， 可以参考官网[https://hexo.io/docs/commands.html](https://hexo.io/docs/commands.html)
个人建议不要把hexo命令都搞懂， 没必要，常用的命令熟悉了，其他的自然也就熟悉了.

