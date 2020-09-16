#  Git日常使用清单

[toc]

每天使用 Git ，但是很多命令记不住。

一般来说，日常使用只要记住下图6个命令，就可以了。但是熟练使用，恐怕要记住60～100个命令。

![img](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015120901.png)

下面是我整理的常用 Git 命令清单。几个专用名词的译名如下。

 - Workspace：工作区
 - Index / Stage：暂存区
 - Repository：仓库区（或本地仓库）
 - Remote：远程仓库

# 一、常用命令

## 常用汇总

- `clone` 克隆**从远程仓库Remote到工作区workspace**

```shell
 # 下载一个项目和它的整个代码历史
 $ git clone [url]
```



-  `checkout` 检出**从本地仓库Repository到工作区workspance**

```shell
# 新建分支并切换到指定分支 
$ git checkout -b [branch-name] [remote-branch]  

# 新建一个分支，并切换到该分支
 $ git checkout -b [branch]
```



- `add` 增加**从工作区workspance到暂存区index**

```shell
 # 添加当前目录的所有文件到暂存区
 $ git add .
 
 # 添加指定文件到暂存区
 $ git add [file1] [file2] ...
```



- `commit` 提交**从暂存区index到本地仓库Repository**

```shell
 # 提交暂存区到仓库区
 $ git commit -m [message]
```



- 分支`branch`

```shell
# 删除本地分支命令行: 
$ git branch -d <BranchName>

# 删除远程分支
$ git branch -dr [remote/branch]
```



# 二、命令汇总

## 新建代码库

 ```bash
 # 在当前目录新建一个Git代码库
 $ git init
 
 # 新建一个目录，将其初始化为Git代码库
 $ git init [project-name]
 
 # 下载一个项目和它的整个代码历史
 $ git clone [url]
 ```

## 配置

Git的设置文件为`.gitconfig`，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

 ```bash
 # 显示当前的Git配置
 $ git config --list
 
 # 编辑Git配置文件
 $ git config -e [--global]
 
 # 设置提交代码时的用户信息
 $ git config [--global] user.name "[name]"
 $ git config [--global] user.email "[email address]"
 ```

## 增加/删除文件

 ```bash
 # 添加指定文件到暂存区
 $ git add [file1] [file2] ...
 
 # 添加指定目录到暂存区，包括子目录
 $ git add [dir]
 
 # 添加当前目录的所有文件到暂存区
 $ git add .
 
 # 添加每个变化前，都会要求确认
 # 对于同一个文件的多处变化，可以实现分次提交
 $ git add -p
 
 # 删除工作区文件，并且将这次删除放入暂存区
 $ git rm [file1] [file2] ...
 
 # 停止追踪指定文件，但该文件会保留在工作区
 $ git rm --cached [file]
 
 # 改名文件，并且将这个改名放入暂存区
 $ git mv [file-original] [file-renamed]
 ```

## 代码提交

 ```bash
 # 提交暂存区到仓库区
 $ git commit -m [message]
 
 # 提交暂存区的指定文件到仓库区
 $ git commit [file1] [file2] ... -m [message]
 
 # 提交工作区自上次commit之后的变化，直接到仓库区
 $ git commit -a
 
 # 提交时显示所有diff信息
 $ git commit -v
 
 # 使用一次新的commit，替代上一次提交
 # 如果代码没有任何新变化，则用来改写上一次commit的提交信息
 $ git commit --amend -m [message]
 
 # 重做上一次commit，并包括指定文件的新变化
 $ git commit --amend [file1] [file2] ...
 ```

## 分支

 ```bash
 # 列出所有本地分支
 $ git branch
 
 # 列出所有远程分支
 $ git branch -r
 
 # 列出所有本地分支和远程分支
 $ git branch -a
 
 # 新建一个分支，但依然停留在当前分支
 $ git branch [branch-name]
 
 # 新建一个分支，并切换到该分支
 $ git checkout -b [branch]
 
 # 新建一个分支，指向指定commit
 $ git branch [branch] [commit]
 
 # 新建一个分支，与指定的远程分支建立追踪关系
 $ git branch --track [branch] [remote-branch]
 
 # 切换到指定分支，并更新工作区
 $ git checkout [branch-name]
 
 # 切换到上一个分支
 $ git checkout -
 
 # 建立追踪关系，在现有分支与指定的远程分支之间
 $ git branch --set-upstream [branch] [remote-branch]
 
 # 合并指定分支到当前分支
 $ git merge [branch]
 
 # 选择一个commit，合并进当前分支
 $ git cherry-pick [commit]
 
 # 删除分支
 $ git branch -d [branch-name]
 
 # 删除远程分支
 $ git push origin --delete [branch-name]
 $ git branch -dr [remote/branch]
 ```

## 标签

 ```bash
 # 列出所有tag
 $ git tag
 
 # 新建一个tag在当前commit
 $ git tag [tag]
 
 # 新建一个tag在指定commit
 $ git tag [tag] [commit]
 
 # 删除本地tag
 $ git tag -d [tag]
 
 # 删除远程tag
 $ git push origin :refs/tags/[tagName]
 
 # 查看tag信息
 $ git show [tag]
 
 # 提交指定tag
 $ git push [remote] [tag]
 
 # 提交所有tag
 $ git push [remote] --tags
 
 # 新建一个分支，指向某个tag
 $ git checkout -b [branch] [tag]
 ```

## 查看信息

 ```bash
 # 显示有变更的文件
 $ git status
 
 # 显示当前分支的版本历史
 $ git log
 
 # 显示commit历史，以及每次commit发生变更的文件
 $ git log --stat
 
 # 搜索提交历史，根据关键词
 $ git log -S [keyword]
 
 # 显示某个commit之后的所有变动，每个commit占据一行
 $ git log [tag] HEAD --pretty=format:%s
 
 # 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
 $ git log [tag] HEAD --grep feature
 
 # 显示某个文件的版本历史，包括文件改名
 $ git log --follow [file]
 $ git whatchanged [file]
 
 # 显示指定文件相关的每一次diff
 $ git log -p [file]
 
 # 显示过去5次提交
 $ git log -5 --pretty --oneline
 
 # 显示所有提交过的用户，按提交次数排序
 $ git shortlog -sn
 
 # 显示指定文件是什么人在什么时间修改过
 $ git blame [file]
 
 # 显示暂存区和工作区的差异
 $ git diff
 
 # 显示暂存区和上一个commit的差异
 $ git diff --cached [file]
 
 # 显示工作区与当前分支最新commit之间的差异
 $ git diff HEAD
 
 # 显示两次提交之间的差异
 $ git diff [first-branch]...[second-branch]
 
 # 显示今天你写了多少行代码
 $ git diff --shortstat "@{0 day ago}"
 
 # 显示某次提交的元数据和内容变化
 $ git show [commit]
 
 # 显示某次提交发生变化的文件
 $ git show --name-only [commit]
 
 # 显示某次提交时，某个文件的内容
 $ git show [commit]:[filename]
 
 # 显示当前分支的最近几次提交
 $ git reflog
 ```

## 远程同步

 ```bash
 # 下载远程仓库的所有变动
 $ git fetch [remote]
 
 # 显示所有远程仓库
 $ git remote -v
 
 # 显示某个远程仓库的信息
 $ git remote show [remote]
 
 # 增加一个新的远程仓库，并命名
 $ git remote add [shortname] [url]
 
 # 取回远程仓库的变化，并与本地分支合并
 $ git pull [remote] [branch]
 
 # 上传本地指定分支到远程仓库
 $ git push [remote] [branch]
 
 # 强行推送当前分支到远程仓库，即使有冲突
 $ git push [remote] --force
 
 # 推送所有分支到远程仓库
 $ git push [remote] --all
 ```

## 撤销

 ```bash
 # 恢复暂存区的指定文件到工作区
 $ git checkout [file]
 
 # 恢复某个commit的指定文件到暂存区和工作区
 $ git checkout [commit] [file]
 
 # 恢复暂存区的所有文件到工作区
 $ git checkout .
 
 # 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
 $ git reset [file]
 
 # 重置暂存区与工作区，与上一次commit保持一致
 $ git reset --hard
 
 # 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
 $ git reset [commit]
 
 # 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
 $ git reset --hard [commit]
 
 # 重置当前HEAD为指定commit，但保持暂存区和工作区不变
 $ git reset --keep [commit]
 
 # 新建一个commit，用来撤销指定commit
 # 后者的所有变化都将被前者抵消，并且应用到当前分支
 $ git revert [commit]
 
 # 暂时将未提交的变化移除，稍后再移入
 $ git stash
 $ git stash pop
 ```

## 其他

 ```bash
 # 生成一个可供发布的压缩包
 $ git archive
 ```



# 三、常规操作



## 添加SSH

```shell
# Copies the contents of the id_rsa.pub file to your clipboard
$ pbcopy < ~/.ssh/id_rsa.pub

# Settings → SSH and GPG keys →  New SSH key
```



## 新建版本库

```shell
# 远程仓库中新建仓库（github）

# 创建目录并新建一个Git代码库
$ git init

# 在版本库中添加示例文件，如README.md文件（不能为空文件，如有文件直接git add .）
$ git add README.md
$ git commit -m "README for this projehub"

# 为版本库添加名为origin的远程版本库
$ git remote add origin git@github.com:ChaseRain/helloworld.git

# 推送至远程仓库 -u表示建立追踪关系
$ git push -u origin master


```



## 创建远程分支

```shell
# 在当前分支下创建dev的本地分支分支
$ git checkout -b dev

# 将dev分支推送到远程
$ git push origin dev  

# 将本地分支dev关联到远程分支dev上   
$ git branch --set-upstream-to=origin/dev 

# 查看远程分支 
$ git branch -a 
```



## 删除远程文件

具体操作步骤如下：

```shell
# 1、预览将要删除的文件
git rm -r -n --cached 文件/文件夹名称 

# 2、确定无误后删除文件
git rm -r --cached 文件/文件夹名称

# 3、提交到本地并推送到远程服务器
git commit -m "提交说明"
git push origin master

# 4、修改本地 .gitignore 文件 并提交（同上）
```



## 合并分支推送

```shell
# 切换到主干
$ git checkout master

# -no-ff 参數可以保存dev分支上的历史记录
$ git merge --no-ff dev

# 先pull避免产生冲突
$ git pull origin maste

# 推送到远程仓库
$ git push origin master
```

## 版本回退

```shell
# 查看提交记录
$ git log

# 方法1
$ git reset --hard dbf5efdb3cd8ea5d576f2e29fe0db1951d0e3e3b
# 强制推送到远程分支, 会抹去远程库的提交信息(不建议)
# git push -f origin master
 
# 方法2
# 回退到指定版本, 需要解决冲突
$ git revert e7c8599d29b61579ef31789309b4e691d6d3a83f
# 放弃回退(加--hard会重置已 commit和工作区 的内容)
$ git reset --hard origin/master 
```

## 解决冲突

```shell
# 查看冲突状态
$ git status

# 修改冲突文集
$ vim readme.txt

# 提交冲突文件
$ git add readme.txt 
$ git commit -m "conflict fixed"

# 带参数的查看分支合并情况
git log --graph --pretty=oneline --abbrev-commit

```


## 储藏功能

```shell
# 命名并储藏到缓存堆栈中
$ git stash save "test-cmd-stash"

# 查看储藏列表
$ git stash list

# 重新应用缓存堆栈中（不删除stash拷贝），指定哪个stash
$ git stash apply 6

# 移除stash，可指定stash
$ git stash drop 6

```








