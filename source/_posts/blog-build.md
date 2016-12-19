---
title: 搭建博客
date: 2016-06-11 04:00:00
categories: 杂七杂八
---

主要时为了方便自己的记忆

安装环境for Mac
git的环境安装跳过
```
安装Node.js
brew install node

验证安装成功:
node -v
npm -v
```
<!--more-->
```
安装Hexo:
npm install hexo -g
```
### 搭建
1. 创建仓库  xyiyy.github.io
2. 创建两个分支 master 和 hexo
```
本地创建一个分支： git branch Branch1
切换到分支Branch1: git checkout Branch1
将新分支发布到github上: git push origin Branch1
在本地删除一个分支: git branck -d Branch1
在github远程端删除一个分支: git push origin :Branch1
```
3. 设置hexo为默认分支(通过此分支来保存源文件)
4. 在本地文件夹下依次执行（hexo分支下）
```
hexo init
npm install
npm install hexo-deployer-git
```
这里可能需要先将之前创建的部分文件提出，否则会被覆盖
5. 修改_config.yml中deploy参数，分支应改为master
6. 依次执行如下步骤，将源文件提交保存
```
git add .
git commit -m "备注"
git push origin hexo
```
7. 将网页部署到github上
```
hexo generate -d
```

### 日常更新
只需要执行搭建中的第6步和第7步

### 机器转移
+ 首先要配置好环境
+ 将项目从github上clone下来
+ 执行第6步，第7步
