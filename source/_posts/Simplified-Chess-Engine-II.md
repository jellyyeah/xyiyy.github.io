---
title: Simplified_Chess_Engine_II [HackerRank Week of Code 27]
date: 2016-12-28 12:55:47
tags: 搜索
categories: HackerRank
---

[题目地址](https://www.hackerrank.com/contests/w27/challenges/simplified-chess-engine-ii)

<!--more-->
题意：给你一个4x4的国际象棋棋盘每方有一个Queen，最多两个Pawns，最多两个Rooks，以及Bishop和Knight加起来最多两个。Queen被吃掉的一方算输。Pawns的第一步不能走两格，没有吃过路兵。Pawns到最后一行除了不能变Queen和Pawns之外，其余都可以变。其余规则和国际象棋一致。白子先走，问六步之内先手能不能赢，否则就算黑赢。  
解法：因为棋盘很小，所以肯定是搜搜搜。  
对于当前一步，优先去判断输赢，注意到这个trick就不会T了，也就是先去找出所有当前局面走了一步之后的所有局面。  
如果对于当前玩家有必胜方案，那么他一定会去走必胜的。  

代码太长，略。
