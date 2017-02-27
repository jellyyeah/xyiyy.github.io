---
title: HackerRank中python的numpy部分的题目
date: 2016-12-28 17:05:43
tags: numpy
categories: HackerRank python

---

本菜鸡近期开始学习CS231n，然后随意找了个地方练了练numpy。
hr上的题目难度似乎都是easy。
大致总结一下，主要用到了以下几个：
<!--more-->
1. array()
2. reshape(),shape
3. transpose() 求转置
4. flatten() 将数组变成一维数组
5. concatenate() 其中axis＝0表示垂直组合，1表示水平组合
6. zeros(),ones()
7. eye() 对角线为1，其余为0，第三个参数为偏移值
8. identity() 对角线为1，其余为0的方阵
9. floor(),ceil(),rint() rint其实就是round
10. sum(),prod() 有一个axis的参数
11. min(),max() 参数同上一条
12. Mean(),var(),std() 算术平均值，算术方差，算术标准差，参数同上
13. dot(),cross() dot(a,b)即为[i,j,k,m] = sum(a[i,j,:] * b[k,:,m])
14. innner() innner(a,b) [i,j,k,m] = sum(a[i,j,:] * b[k,m,:])
15. outer() outer(a,b)先将a,b分别占成一维行向量和一位列向量，然后做一个乘法
16. poly() 根据根求多项式系数
17. roots() 根据多项式求根
18. polyint(),polyder() 求积分和微分
19. polyval(p,x) 求x位置的多项式的值
20. polyfit(x,y,n) 用最小二乘法去拟合的最高为n次的多项式
21. polyadd(),polysub(),polymul(),polydiv()
22. linalg.det() 求行列式的值
23. linalg.eig() 求行列式的特征值以及特征向量
24. linalg.inv() 求逆矩阵  

剩下的可以参考[此处](https://docs.scipy.org/doc/numpy/reference/routines.linalg.html)

### Arrays

```python
import numpy as np
print(np.array(list(reversed(input().split())),float))
```

### Shape and Reshape

```python
import numpy as np
print(np.array(input().split(),int).reshape(3,3))
```

### Transpose and Flatten
```python
import numpy as np
n,m = [int(x) for x in input().split()]
my_array = np.array([input().split() for i in range(n)], int)
print(my_array.transpose())
print(my_array.flatten())
```
### Concatenate
```python
import numpy as np
n,m,p = map(int,input().split())
array_A = np.array([input().split() for i in range(n)],int)
array_B = np.array([input().split() for i in range(m)],int)
print(np.concatenate((array_A,array_B),axis=0))
```

### Zeros and Ones
```python
import numpy as np
N = tuple(map(int,input().split()))
print(np.zeros(N,dtype=np.int),np.ones(N,dtype=np.int),sep='\n')
```

### Eye and Identity
```python
import numpy as np
n,m = map(int,input().split())
print(np.eye(n,m))
```
### Array Mathematics
```python
import numpy as np
n,m = map(int,input().split())
A = np.array([input().split() for i in range(n)],int)
B = np.array([input().split() for i in range(n)],int)
print(A + B,A - B,A * B, A // B, A % B,A ** B,sep = '\n')
```

### Floor,Ceil and Rint
```python
import numpy as np
A = np.array(input().split(),float)
print(np.floor(A),np.ceil(A),np.rint(A),sep='\n')
```

### Sum and Prod
```python
import numpy as np
n,m = map(int,input().split())
A = np.array([input().split() for i in range(n)],int)
print(np.prod(np.sum(A,axis = 0)))
```

### Min and Max
```python
import numpy as np
n,m = map(int,input().split())
A = np.array([input().split() for i in range(n)],int)
print(np.max(np.min(A,axis = 1)))
```

### Mean,Var and Std
```python
import numpy as np
n,m = map(int,input().split())
A = np.array([input().split() for i in range(n)],int)
print(np.mean(A,axis = 1),np.var(A,axis = 0), np.std(A),sep = '\n')
```
### Dot and Cross
```python
import numpy as np
n = int(input())
A = np.array([input().split() for i in range(n)],int)
B = np.array([input().split() for i in range(n)],int)
print(np.dot(A,B))
```

### Inner and Outer
```python
import numpy as np
A,B = [np.array(input().split(),int) for i in range(2)]
print(np.inner(A,B),np.outer(A,B),sep='\n')
```

### Polyomials
```python
import numpy as np
print(np.polyval(list(map(float,input().split())),float(input())))
```
### Linear Algebra
```python
import numpy as np
n = int(input())
print(np.linalg.det(np.array([input().split() for i in range(n)],float)))
```
