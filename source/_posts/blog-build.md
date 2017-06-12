---
title: 搭建博客
date: 2016-06-11 04:00:00
categories: 杂七杂八
---

主要时为了方便自己的记忆

安装环境for Mac
git的环境安装跳过
```bash
安装Node.js
brew install node  
验证安装成功:
node -v
npm -v
```
<!--more-->
```bash
安装Hexo:（现在hexo3还要另外装server）
npm install hexo -g
```
### 搭建
1. 创建仓库  xyiyy.github.io
2. 创建两个分支 master 和 hexo
```bash
本地创建一个分支： git branch hexo
切换到分支hexo: git checkout hexo
将新分支发布到github上: git push origin hexo
```
3. 设置hexo为默认分支(通过此分支来保存源文件)
4. 在本地文件夹下依次执行（hexo分支下）
```bash
hexo init
npm install
npm install hexo-deployer-git
```
这里可能需要先将之前创建的部分文件提出，否则会被覆盖
5. 修改_config.yml中deploy参数，分支应改为master
6. 依次执行如下步骤，将源文件提交保存
```bash
git add .
git commit -m "备注"
git push origin hexo
```
7. 将网页部署到github上
```bash
hexo generate -d
```

### 日常更新
只需要执行搭建中的最后两步

### 机器转移
+ 首先要配置好环境
+ 将项目从github上clone下来
+ 执行最后两步
