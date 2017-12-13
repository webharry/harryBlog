---
title: svn常用指令总结
date: 2017-12-13 17:33:18
tags:
    - svn
    - frontend
    - '版本控制'
---
### 常用命令总结
#### svn add
* 将未版本化文件/新建文件提交到版本库.
```shell
svn add 文件名/目录名
```
* 提交文件：
```shell
svn add a.js
```
* 提交目录：
```shell
svn add dist
```
* 或者进入到待提交目录下：
```shell
svn add .
```
svn add *会忽略已经在版本控制之下的目录，需要提交工作副本中的所有未版本化文件，使用svn add --force命令，可以递归的提交未版本化文件，包括忽略文件，慎用。
```shell
svn add *
svn add * --force
```
--force表示递归的提交目录下的所有文件。
#### svn commit 
* 提交命令.将工作副本中的更改提交到版本库
```shell
svn commit -m '备注'
```
* 缩写
```shell
svn ci -m '备注'
```
#### svn update
* 更新命令。将版本库的文件更新到工作副本。
```shell
svn update
```
* 缩写：
```shell
svn up
```
#### svn log
* 查看提交日志。
```shell
svn log
```
#### svn revert
* 将文件回滚到提交前，如果有误操作，可以使用svn revert
```shell
svn revert
```
#### tag/branch
##### 创建tag
* 在产品已经开发完准备上线时，就可以创建tag做版本标记，发布给客户用。
进入到工作副本的trunk目录下：
```shell
svn copy http://svn_server/xxx_repository/trunk http://svn_server/xxx_repository/tags/release-1.0 -m "1.0 released"
```
##### 创建branch分支
在本期开发中，遇到紧急需求需要临时上线，这时可以创建一个branch分支，进行紧急开发上线。  
* 在工作副本中创建分支：

```shell
root@runoob:~/svn/runoob01# ls
branches tags     trunk
root@runoob:~/svn/runoob01# svn copy trunk/ branches/br_001
回车：
A         branches/br_001
```
* 提交新增的分支到版本库：
```shell
root@runoob:~/svn/runoob01# svn ci -m 'add branches/br_001'
Adding         branches/br_001
```
* 切换到trunk,执行svn update,然后将br_001分支合并到trunk主干：
```shell
root@runoob:~/svn/runoob01/trunk# svn ../branches/br_001/
```
* 查看主干：
```shell
root@runoob:~/svn/runoob01/trunk# ll
```

提交到版本库后，
* 删除branches分支：
```shell
root@runoob:~/svn/runoob01#  svn rm svn://191.111.111.11/xxx_repository/branches/br_001
```
* 删除tag:
```shell
root@runoob:~/svn/runoob01#  svn rm svn://191.111.111.11/xxx_repository/tags/release_001
```
### svn status
查看工作副本中的文件状态，缩写svn st.
* 我们新建文件index.html，演示下svn st的使用。
```shell
root@runoob:~/svn/runoob01# svn st
?        index.html
```
* ? 问号表示该文件没有在版本控制下,需要先使用svn add命令添加到版本控制中
* A 表示文件已经成功添加到版本控制中，然后再svn up提交到版本库
* M 表示本地文件有修改
* C 表示从版本库更新时与本地工作副本有冲突，必须手动解决冲突
* ！表示文件已在版本控制下，但已丢失，或文件不完整
* D 表示删除了工作副本中的文件，提交到版本库后才算删除成功
* I 表示文件不在版本控制下，在使用svn add,,svn ci命令时会忽略文件，不操作。