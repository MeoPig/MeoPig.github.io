---
title: 【Git】
date: 2022-01-16 14:08:56
tags: Git
---
# Git

## git是什么
  git是分布式版本控制系统
特点：解决多人同时开发的代码问题，也可以找回历史代码。  
原理：一台充当服务器的电脑或者Github网站，其他人可以从这个服务器仓库中拉取一份代码到自己的电脑上，用户可以自行在自己电脑上对代码操作，
操作结束后可以向服务器或者Github推送提交，也可以从仓库中拉取别人的提交。
## git基本操作
### 1. 创建版本库  
在所选目录下使用：git init ，即可对该目录下的文件使用git管理。  
执行命令后会在目录下生成一个 .git 隐藏文件夹，代表成功。
![](Git_1.png)
### 2. 版本创建与回退
* 加入我在在所选目录下创建一个文件code.text,并在里面写了first内容
* 使用下两条命令可以创建一个版本  
`
git add code.txt    (git add命令可将该文件添加到暂存区)`  
`  
git commit -m '版本1' (git commit用来创建版本 ''内容表示对版本的说明)  
`  
![](Git_2.png)
* 使用`git log` 可以查看版本记录,commit后面代表版本的序列号.
![](Git_3.png)
* 此时我又在code.txt中输入了second内容，并继续采用add和commit命令创建了版本2，那么此时我对于code.txt就有了两个版本
每个版本保存了当时创建版本时文件所存储的信息（可以理解为文件记录点），后一版本只记录较上一版本我修改了什么，并不是完整的保存整个文件。
![](Git_4.png)
* 现在若想要回到前一个版本1，可以使用：  
`git reset --hard HEAD^`  
其中HEAD表示当前最新版本，HEAD^表示当前版本的前一个版本，HAED^^表示当前版本的前两个版本。  
也可以使用HEAD~1表示当前版本前一版本，HEAD~100表示回到前100。  
执行命令后code.txt将回到版本1时的状态,只有个first，版本2也没有被删除.
![](Git_5.png)
* 现在若想再回到版本2，则可以：  
`git reset --hard 版本序列号`
![](Git_6.png)
* 如果不小心把终端关闭了，无法再看到之前版本2的序列号则可以执行：  
`git reflog`
![](Git_7.png)
* 查看当前git状态,看看工作区中有没有什么尚未暂存以备提交的变更或者新增加的未跟踪（未add commit的）文件。  
`git status`
### 3. 工作区和暂存区  
 工作区：我们刚才创立的目录git_test就叫工作区。  
 版本库：.git隐藏目录就是版本库,git的版本库中有很多东西，最重要的时暂存区，还有git为我们自动创建的第一个分支Master，以及指向master的一份指针HEAD。git add就是将修改放到暂存区中。  
 git commit就是往master分支上提交更改。可以简单的理解为，需要提交的文件修改通通放到暂存区中，然后一次性提交暂存区的所有修改。  
 <font color="red">我创立的文件在工作区，此时我执行add，该文件存放到了暂存区，我对该文件又进行了修改并commit了，此时修改存到了版本库中。</font>
 ![](Git_8.png)
 

### 4. 管理修改  
git管理的文件修改，<font color="red">它只会提交暂存区的修改来创建版本</font>。

### 5. 撤销修改  
`git checkout -- 文件名`
此命令可以用来丢弃工作区的改动。
`git reset HEAD filename`
此命令可以把暂存区的修改撤销掉，重新放回工作区。
![](Git_9.png)
### 6. 对比文件的不同
* 对比工作区和某个版本中文件的不同。  
`git diff HEAD -- filename`  
比如我在工作区中往code.txt中新加入了different,执行命令后则可以看到刚才的修改与某个版本相比增加了什么。
![](Git_10.png)
* 对比两个版本中文件的不同。  
`git diff HEAD HEAD^ -- filename`  
filename代表要对比两个版本中的哪一个文件。
![](Git_11.png)

### 7. 删除文件  
`git rm filename`  
从版本中删除某文件,并再次commit创建删除记录。  

## git操作命令集合
```
单人操作:
1.sudo apt-get intall git ,安装git ,并创建git密码
2.git, 查看安装结果,有提示则证明安装成功
3.git init , 创建本地仓库
4.git config user.name '张三', 配置git提交的用户名
5.git config user.email '123@qq.com', 配置git提交的邮箱
6.git status, 查看工作区的文件状态
7.git add 文件名, 添加指定文件名到暂存区
8.git add . 添加所有的改动文件到暂存区
9.git commit -m '版本信息描述',  添加当前版本说明
10.git commit -am '版本信息', 直接添加到暂存区并提交到git仓库
11.git log, 查看详情历史版本
12.git reflog, 查看简单历史版本
13.git reset --hard HEAD^ 回退到上个版本
14.git reset --hard HEAD~1回到上个版本
15.git reset --hard 版本号,  跳转到指定的版本号
16.git checkout 文件名,  撤销指定文件的修改,工作区
17.git reset HEAD 文件名,  撤销暂存区的代码
19.git diff HEAD HEAD^ -- 文件名,对比版本库
20.rm 文件名, 删除文件,通过下面21行命令撤销
21.git checkout -- 文件名, 撤销删除
22.git rm 文件名, 删除本地文件, 并提交,通过下面的23行命令撤销
23. git reset --hard HEAD^,撤销

多人开发:
23.git clone 地址, 克隆远程的代码到本地
24.git push, 推送到远程仓库
25.git config --global  credential.helper cache 十五分钟有效期
26.git config  credential.helper 'cache --timeout==3600' 一个小时有效期
27.git config --global credential.helper store 长期有效
28.git pull ,拉取远程代码到本地目录


标签
29.git tag -a 标签名 -m '标签描述v1.0',  本地标签
30.git push origin 标签名, 将本地标签版本推送到远程端
31.git tag -d 标签名,  删除本地标签
32.git push origin --delete tag 标签名, 删除远端的标签名


分支
33.git branch, 查看当前分支
34.git checkout -b 分支名, 切换到指定分支
35.git push -u origin 分支名,  推送本地分支跟踪远程分支
36.git checkout master/dev 切换到master主分支/子分支
37.git merge 分支A, 合并指定分支A到主分支中



```
------
## git分支管理

### 1. 概念及作用
![](Git_12.png)
![](Git_13.png)

### 2. 创建与合并分支

  #### (1)概念：  
  git会将我们之前每次提交的版本串成一条时间线，这条时间线就是一个分支。在git中，这个分支叫做主分支，即master分支。  
  对于HEAD 严格来说不是指向提交，而是指向master，再由master指向提交。
  ![](14.png)
  ![](15.png)
  ![](16.png)
  ![](17.png)
  ![](18.png)
  #### (2)分支的基本操作
* (1)查看当前有几个分支并看到在哪个分支下工作
    `git branch` 
* (2)创建一个新分支dev并切换到上面工作
    `git checkout -b dev`
    此时HEAD指向dev,后续的版本操作都是在dev下操作，master将不会移动，依然指在原先位置。
  ![](19.png)
* (3)将新分支合并到master上
    `git merge dev`
    这条命令使用的是快速合并，也就是直接把master指向dev的当前提交，所以合并速度非常快。  
    合并完成后，就可以放心的执行下列命令删除dev分支。
    `git branch -d dev`
  #### (3)分支操作命令集合
  ![](20.png)

### 3. 解决冲突
  当master和dev都对某一文件进行了修改并提交了版本，这时候合并就会冲突。  
  解决方法是在master中对文件进行手动修改并提交新版本，此时的情况就会变成下图样子：
  ![](21.png)
  此时便可以删除dev。

### 4. 分支管理策略
  通常，合并分支时，如果可能，git会用fast forward模式进行快速合并，但此时不能执行快速合并且合并时没有冲突，这时候合并之后会做一次新的提交(包含合并前两个分支的修改内容)。  
  提交时会出现弹窗让你输入提交说明信息。  
  合并结束后可以删除dev。
### 5.Bug分支
  当我们遇到Bug需要修复时，可以为bug建一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。  
  在实际工作中，我们在做手头工作但是接到了修复Bug的任务，不得不停下来当前任务，此时可以用 `git stash`将当前的工作现场存储起来以便修复Bug后继续手头工作.  
  修复Bug过程：
  1. 首先要确定在哪个分支上修复Bug,假如在master上，就从master创建临时分支。 
  2. 在临时分支中修复bug，并进行提交。
  3. 切换回master，采用禁止快速合并策略进行合并  
    `git merge --no-ff -m '说明信息' 要合并的分支`
  4. 删除Bug临时分支，可以回到原先环境干活，采用   
   `git stash list` 看看保存过的工作现场。  
    采用`git stash pop`便可以回到现场.

