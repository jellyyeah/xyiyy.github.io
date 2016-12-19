---
title: python3下的django连接mysql
date: 2016-06-13 14:00:00
categories: 杂七杂八
---

对于python2而言，使用MYSQLdb连接即可   
而对于python3，则需要另外的操作，我利用了pymysql来连接，用法非常简单。

#### 安装
```
pip install pymysql
```
#### 测试
在terminal下打开用python连接测试即可验证安装是否成功，网上有很多

连接django也比较简单，在settings中的database部分如下配置：
<!--more-->
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'test',
        'USER': 'root',
        'PASSWORD': '12345',

    }
}
```
这里，我们使用了名为test的数据库，需要现在mysql中创建好。   
除此之外在__init__中添加如下：
```
import pymysql
pymysql.install_as_MySQLdb()
```
然后在manage.py中输入：
```
migrate
```
最后在mysql中查看，即可发现django中的数据表已经在mysql中生成了。


#### 参考网址
1. http://www.cnblogs.com/xwang/p/3727741.html
2. http://www.cnblogs.com/fengri/articles/django5.html
