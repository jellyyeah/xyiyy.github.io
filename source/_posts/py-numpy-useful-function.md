---
title: numpy中的一些常用的函数
date: 2017-02-24 15:40:42
tags: numpy
categories:
- python
---

由于自己太过健忘，所以记一些平时要用然后又容易忘记的函数

<!--more-->

#### argsort
返回数组值从小到大的索引值
#### argmax/argmin
返回数组值最大/小的索引
#### bincount(X,weight,minlength)
统计每个整数数出现的次数  
weight是用来给次数带上权值的  
minlength是用来指定最小的数的
#### concatenate((X,...,Z),axis)
组合拼接  
axis＝0表示竖直组合，1表示水平组合

#### hstack,vstack
组合

#### hsplit,vsplit
分割

#### linspace(start,end,num=50)
返回start到end之间num数目的数，均匀有序的。数组

#### norm()
求范数，具体哪个范数可以根据参数设定。默认的为平开和开根号，2范数

#### random.choice(a,size＝None,replace=True,p=None)
a可以是一位数组或是整数，数组的话，表示在数组内的随机，数字的话在arange(a)中随机  
size就是size的意思  
replace表示是否可重复  
数组中每个数对应的被选中的概率
#### ravel()
讲数组展平，变成一维的。展平的方式可以根据参数定义，要用的时候可查，默认的是优先展平最右边的维度

#### split(X,indices_or_sections,axis)
表示将数组切片。如果第二个参数是整数，则必须能够整除，第二个参数也可以传入一维的数组，用来表示在哪几个位置进行切片的操作，返回的结果在list中  
同array_split()
