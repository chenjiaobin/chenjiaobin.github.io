---
title: git版本工具
date: 2017-12-14 10:14:00
tags: 
- git
- 版本管理工具
categories:
- 构建工具
---
git作为当今比较流行的版本管理工具，对于我们每个开发人员来说都是必须懂的一个科目<!--more-->git之所以令人费解，主要是因为它比svn等传统的版本管理工具多引入了一个*暂存区Stage* 的概念
### 区
1. 工作区
2. 暂存区(Stage)
3. 本地仓库
4. 远程仓库
他们每次进入一个区成功后会产生一种状态，在加上最初始的一种状态，总共有5中状态
### 状态
1. 未修改（origin）
2. 已修改（modified）
3. 已暂存（staged）--git add .
4. 已提交（commited）--git commit -m ""
5. 已推送（pushed）
### 查看更改的内容git diff
* 当我们修改的文件还没提交到暂存区的时候（git add .）,我们可以通过`git diff 文件名`查看文件前后添加了那些内容，这是查看工作区和暂存区的差异
* 当我们修改的文件已经提交到暂存区的时候（git commit .）,我们需要通过`git diff --cashed`查看文件前后添加了那些内容，这个是查看暂存区和本地仓库的差异
* 当我们将修改的文件提交到远程服务器的时候（git push）,我们需要通过`git diff master origin/master`查看差异，这是本地仓库和远程仓库之间的比较
### git的4个阶段的撤销更改
* 在工作区修改了东西还没有执行`git add .`,这时候我们的修改还没有提交到暂存区，可以通过`git checkout .`或`git reset --hard`进行撤销操作
* 如果已经执行了`git add .`,未执行`git commit`,那么这时候可以通过`git reset`或`git checkout .`或`git reset --hard`进行撤销操作
* 如果已经执行了`git commit`,未执行`git push`,那么需要执行`git reset --hard origin/master`,意思就是本地仓库被污染了，那么我就从远程仓库把老的代码取回来
* 如果执行`git push`,那么需要通过执行`git reset --hard HEAD^`,在强制push到远程仓库就好了`git push -f`
