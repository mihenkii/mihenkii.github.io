title: "python 环境搭建"
date: 2017-10-23 23:04:57
tags: "pyenv virtualenv"
---


#### 准备

> pyenv : 管理多个Python版本， 是从rbevn(管理多个ruby版本)改造来的
> virtualenv: 管理工作空间的python环境， 可以让每个app的python环境相互隔离, 相当于给每个app都复制一份python

#### 安装
> pyenv 已经集成了 virtualenv， 所以不需要单独安装virtualenv了.

安装pyenv比较简单

``` sh
# 下载到本地:
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
# 设置环境变量
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
# 加载环境变量
source ~/.bash_profile

```

使用

``` sh
pyenv install --list #查看可安装python版本
pyenv install 2.7.12
pyenv local 2.7.12 # 将本地设置成2.7.12版本
pyenv global 2.7.12 # 将系统设置成2.7.12版本
pyenv local system # 将本地设置成系统默认版本
```

卸载 pyenv

```
rm -rf $(pyenv root)
```

#### virtualenv

``` sh
pyenv virtualenv 2.7.12 VENV27 # 将会在pyenv的version目录下新建一个VENV27的python环境

pyenv activate VENV27 # 使用VENV27环境
pyenv deactivate VENV27 # 退出VENV27环境
```

#### 设置pip源
> 国内访问pip默认源超时严重， 建议设置阿里或豆瓣的pip源

```sh
vim ~/.pip/pip.conf
[global]
    trusted-host =  mirrors.aliyun.com
    index-url = http://mirrors.aliyun.com/pypi/simple
```
