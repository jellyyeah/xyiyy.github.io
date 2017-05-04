---
title: Beam Search算法
date: 2017-04-22 15:47:00
tags: 搜索
categories:  算法
---
Beam search, 简单来讲，那就是个bfs，主要还是为了解空间过大时而引入的一种启发式搜索。

它主要是因为bfs在逐层搜索的时候，可能随着层数的增加，而内存中不足以存下如此多的状态，而采取了一种剪枝。它对每一层的size做了一个限制，然后如果下一层带扩展的状态超过了这个size的限制，那就选取估价函数值最好的若干个，然后这样不断继续。

嗯，其它也没啥了，跟bfs差不多，搜到目标状态就可以停了，当然，也很有可能最终搜不到目标状态。

然而我还不会用。。。。