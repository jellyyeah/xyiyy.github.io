---
title: Mac下多版本python环境搭建
date: 2016-06-12 08:00:00
categories: 杂七杂八 python
---

### Anaconda
最近发现Anaconda很好用。
这里有篇关于如何使用的[文章](http://www.jianshu.com/p/2f3be7781451#)


### pyenv
[pyenv](https://github.com/yyuu/pyenv)是多版本的python管理器，可以让多个版本的python环境共存。如pypy，python2,python3等等

安装
```bash
$ brew install pyenv
```
或者
```bash
$ git clone https://github.com/yyuu/pyenv.git ~/.pyenv  
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile  
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile  
$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile  
$ source ~/.bash_profile
```
<!-- more -->
在安装完成之后，可以利用pyenv安装其它版本的python
```bash
$ pyenv install --list //查看可安装的python版本
$ pyenv install 3.4.4 安装python3.4.4
```
查看已经安装了的python版本
```bash
$ pyenv versions
```
将某一版本切换为全局
```bash
$ pyenv global 3.4.4
```
对当前目录设置为某一版本
```bash
$ pyenv local 3.4.4
```
### 利用virtualenv创建虚拟python环境
pyenv-virtualenv插件安装
```bash
$ git clone https://github.com/yyuu/pyenv-virtualenv.git ~/.pyenv/plugins/pyenv-virtualenv   
$ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile
$ source ~/.bash_profile
```
创建一个3.4.4的虚拟环境
```bash
$ pyenv virtualenv 3.4.4 env344
```


### 切换使用
```bash
$ pyenv activate env344  
$ pyenv deactivate
```

### 参考网址
1. [http://www.myexception.cn/perl-python/1905836.html](http://www.myexception.cn/perl-python/1905836.html)
2. [http://www.open-open.com/lib/view/open1456751878453.html](http://www.open-open.com/lib/view/open1456751878453.html)
3. [http://www.it165.net/pro/html/201405/13603.html](http://www.it165.net/pro/html/201405/13603.html)
4. [http://www.jianshu.com/p/fa2b7edc5d01](http://www.jianshu.com/p/fa2b7edc5d01)
