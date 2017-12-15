---
title: git
date: 2017-12-15 20:31:23
tags:
    - frontend
    - '构建生态'
    - git
---
### git工作流程   

区别svn的集中式，git是分布式的，
工作流程见图：
![工作流程图2](https://upload.cc/i/HUw2oj.jpeg)  
git
git工作区、版本库、暂存区关系：
![工作原理图](https://upload.cc/i/tk4uTG.jpeg)  

这里有几个概念：
* git有工作区和版本库两个概念。而暂存区index（或者stage）属于版本库的一块。
* 执行git commit操作时，暂存区的目录树写到版本库（对象库）中，master分支也会做相应更新。即master指向暂存区提交的目录树。  
* git push操作推送到共享版本库（对象库）。

### 常用命令
#### git pull 
从版本库库（远程服务器）更新代码到本地工作副本
```shell
git pull    
```
#### git add .
添加文件到暂存区
```shell
git add .   
```
#### git commit
提交
```shell
git commit -m '备注'    
```
#### git push
将文件推到远程服务器的master主分支上
```shell
git push origin master      
```
#### git stutes
查看文件状态
```shell
git stutes    
```
#### git checkout branch 
切换到分支branch
```shell
git checkout branch     
```
#### git merge branch
将分支branch合并到master主干上
```shell
➜  test git:(master)git merge branch   
```
#### gti branch -D branchNamed
删除分支branch1
```shell
gti branch -D branch1 
```

### 总结
* 与svn相比，git在分支上特别好用，svn 分支只是复制出另外一个文件，是多出一个目录，虽然最后也可以合并branch。而git可以在相同目录下切换分支，分支上开发互不影响，最后合并分支即可。