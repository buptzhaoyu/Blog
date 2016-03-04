# Git教程笔记  

> **教程地址:** [http://www.liaoxuefeng.com](http://www.liaoxuefeng.com)  
> **Note By:** Daniel Zhao

***


[TOC]

___
## 1. 安装Git  
 - `sudo apt-get install git`: Linux安装方法  
 - `brew install git`: MAC安装homebrew后，运行此命令安装  
 
 ***
 
## 2. 创建版本库  
 - `git init`: 初始化一个Git仓库  
 - 添加文件到Git仓库，分两步：
	 1. 第一步，使用命令`git add <file>`,注意，可反复多次使用，添加多个文件 
	 2. 第二步，使用命令`git commit -m "描述"`，完成  

***

## 3. 时光机穿梭  
 - `git status`: 掌握工作区状态(git s)  
 - `git diff`: 查看修改内容  

### 版本回退  
 - `git log` (`git lg` or `git llg`): 查看历史纪录
 - `git log --pretty=oneline`: 在一行简明显示历次纪录
 - `git reset --hard commit_id`: 恢复到指定commit id的版本
 - `HEAD`指向当前版本，`HEAD^`指向前一个版本，以此类推
 - `git reflog`: 查看命令历史，用于回到未来版本  

### 管理修改
- `git diff HEAD -- readme.txt`: 查看工作区和版本库里最新版本的区别
- 每次修改，如果不`add`到暂存区就不会被`commit`

### 撤销修改
- `git checkout -- file`: 丢弃工作区的修改，将`file`回到最近一次`git commit`或`git add`的状态
- `git reset HEAD file`: 撤销暂存区的修改，将`file`放回到工作区中

### 删除文件
- `git rm` + `git commit`: 删除文件
- `git checkout -- file`: 误删文件之后，用版本库的版本替换工作区的版本，“一键还原"

***

## 4. 远程仓库
### 添加远程库
1. 创建SSH Key
`$ ssh-keygen -t rsa -C "youremail@example.com"`（若在.ssh目录下有`id_rsa`和`id_rsa.pub`文件，可忽略此步骤）
2. 登陆GitHub，打开个人设置，将`id_rsa.pub`文件内容加入到SSH Key中
3. 在GitHub上新建仓库，“Creat a new repo"
4. `git remote add origin git@server-name:path/repo-name.git`: 关联远程仓库
5. `git push -u origin master`: 第一次推送`master`分支的所有内容
6. `git push origin master`: 此后推送，不需要输入`-u`参数

### 从远程库克隆
- `git clone git@github.com:your_name/repo_name.git`: 克隆远程仓库
- Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快

___

## 分支管理
### 创建与合并分支
- `git branch`: 查看分支
- `git branch <name>`: 创建分支
- `git checkout <name>`: 切换分支
- `git checkout -b <name>`: 创建＋切换分支
- `git merge <name>`: 合并`name`分支到当前分支
- `git branch -d <name>`: 删除分支

### 解决冲突
- `git log --graph`: 查看分支合并图
>当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

### 分支管理策略
> 合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

### Bug分支
- `git stash`: 保存当前工作现场
- `git stash pop`: 回到工作现场

### Feature分支
> 开发一个新feature，最好新建一个分支；
> 如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

### 多人协作
- `git remote -v`: 查看远程库信息
- 本地新建的分支如果不推送到远程，对其他人不可见
- `git push origin branch_name`: 从本地推送分支，若失败，先用`git pull`抓去远程的新提交
- `git checkout -b branch_name origin/branch_name`: 在本地创建和远程分支名称相同的分支
- `git branch --set-upstream branch_name origin/branch_name`: 建立本地分支和远程分支的关联
- `git pull`抓去远程分支有冲突时，先解决冲突

___

## 标签管理
### 