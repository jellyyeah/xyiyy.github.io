---
title: Mac下多版本python环境搭建
---
### pyenv
[pyenv](https://github.com/yyuu/pyenv)是多版本的python管理器，可以让多个版本的python环境共存。如pypy，python2,python3等等

Install it
```
$ brew install pyenv
```
Once you have it, you can install different versions of python and choose which one you can use.  
<!-- more -->
Example:
```
$ pyenv install 3.4.4
```
You can check the versions you have installed with:
```
$ pyenv versions
```
And you can switch between python versions with the command
```
$ pyenv global 3.4.4
```
Also you can set a python version for the current directory with:
```
$ pyenv local 3.4.4
```

### 参考网址
1. [http://www.myexception.cn/perl-python/1905836.html](http://www.myexception.cn/perl-python/1905836.html)
2. [http://www.open-open.com/lib/view/open1456751878453.html](http://www.open-open.com/lib/view/open1456751878453.html)
3. [http://www.tuicool.com/articles/NBBbme](http://www.tuicool.com/articles/NBBbme)
4. [http://www.it165.net/pro/html/201405/13603.html](http://www.it165.net/pro/html/201405/13603.html)
5. [http://www.jianshu.com/p/fa2b7edc5d01](http://www.jianshu.com/p/fa2b7edc5d01)
