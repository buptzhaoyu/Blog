# Git教程笔记  
![GitHub Mark](http://github.global.ssl.fastly.net/images/modules/logos_page/GitHub-Mark.png "GitHub Mark")  
> **教程地址:** [http://www.liaoxuefeng.com](http://www.liaoxuefeng.com)  
> **Note By:** Daniel Zhao  


[TOC]  


---

## 1. 安装Git  
- `sudo apt-get install git`: Linux安装方法  
- `brew install git`: MAC安装homebrew后，运行此命令安装  

---

## 2. 创建版本库  
- `git init`: 初始化一个Git仓库  
- 添加文件到Git仓库，分两步：  
	 1. 第一步，使用命令`git add <file>`,注意，可反复多次使用，添加多个文件  
	 2. 第二步，使用命令`git commit -m "描述"`，完成  

---

## 3. 时光机穿梭  
- `git status`: 掌握工作区状态(`git s`)  
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

---

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

---

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

> 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。  

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

---

## 标签管理  
> Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针。  

### 创建标签  
- `git tag <name>`: 打一个新标签，在此之前需要切换到需要打标签的分支上  
- `git tag`: 查看所有标签  
- `git tag -a <tagname> -m "blablabla..."`: 指定标签信息  
- `git tag -s <tagname> -m "blablabla..."`: 用PGP签名标签  

### 操作标签  
- `git push origin <tagname>`: 推送一个本地标签  
- `git push origin --tags`: 推送全部未推送的本地标签  
- `git tag -d <tagname>`: 删除一个本地标签  
- `git push origin :refs/tags/<tagname>`: 删除一个远程标签  

---

## 使用GitHub  
> - 在GitHub上，可以任意Fork开源仓库  
> - 自己拥有Fork后的仓库的读写权限  
> - 可以推送pull request给官方仓库来贡献代码  

---

## 自定义Git  
- `git config --global color.ui true`: 让Git适当显示不同的颜色  
### 忽略特殊文件  
> - 忽略某些文件时，需要编写`.gitignore`  
> - `gitignore`文件本身要放到版本库里，并且可以对`.gitignore`做版本管理  

### 配置别名
- `git config --global alias.<blabla> <command>`: 配置别名给对应的`command`  
- 每个仓库的Git配置文件都放在`.git/config`文件中  
- 当前用户的Git配置文件放在用户祝目录下的一个隐藏文件`.gitconfig`中  

### 搭建Git服务器  
> 搭建Git服务器需要准备一台运行Linux的及其，强烈推荐用Ubuntu或Debian，这样，通过几条简单的`apt`命令就可以完成安装。  
> 1. __安装`Git__  
> `$ sudo apt-get install git`  
> 2. __创建一个`git`用户，用来运行`git`服务__  
> `$ sudo adduser git`  
> 3. __创建证书登录__  
> 收集所有需要登录的用户的公钥，就是他们自己的`id_rsa.pub`文件，把所有公钥导入到`/home/git/.ssh/authorized_keys`文件里，一行一个。  
> 4. __初始化Git仓库__  
> 先选定一个目录作为Git仓库，假定是`/srv/sample.git`，在`/srv`目录下输入命令：  
> `sudo git init --bare sample.git`  
> Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为`git`:  
> `$ sudo chown -R git:git sample.git`  
> 5. __禁用shell登录__  
> 出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑`/etc/passwd`文件完成。找到类似下面的一行：  
> `git:x:1001:1001:,,,:/home/git:/bin/bash`  
> 改为：  
> `git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell`  
> 这样，`git`用户可以正常通过ssh使用git，但无法登录shell，因为我们为`git`用户指定的`git-shell`每次一登录就自动退出。  
> 6. __克隆远程仓库__  
> 现在，可以通过`git clone`命令克隆远程仓库了，在各自的电脑上运行：  
> ```
> $ git clone git@server:/srv/sample.git  
> Cloning into `sample`...  
> warning: You appear to have cloned an empty repository  
> ```  
> - 要方便管理公钥，用__Gitosis__  
> - 要像SVN那样变态地控制权限，用__Gitolite__  

---

![Git](http://git-scm.com/images/logo@2x.png)  
> [Git Official](http://git-scm.com)
