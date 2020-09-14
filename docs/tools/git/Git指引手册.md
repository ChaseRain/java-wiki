# Git实操指引

## **1 创建远程仓库，并拉到本地**

创建远程仓库的时候默认是创建master分支的，因此拉下来的项目也处于master分支。

```shell
$ git clone ...
```

##  **2 开发新功能**

- 假如开发新功能a，在master分支创建新功能分支a

  ```shell
  $ git checkout master 
  $ git checkout -b feature/a
  ```

  

- **如果有必要**，将该功能分支推送到远程

  ```shell
  $ git push origin feature/a:feature/a
  ```

  

- **如果有必要**，成员可将该分支拉下来

  ```shell
  $ git fetch origin feature/a:feature/a
  ```

  

## **3 完成新功能**

- 新功能完成之后需要将feature分支合并到master分支，并push到远程仓库(在push之前，建议先pull一下，将本地的feature分支内容更新到最新版本，再push，避免线上版本与你commit时候文件内容产生冲突)

  ```shell
  $ git checkout master # -no-ff 参數可以保存feature/a分支上的历史记录 
  $ git merge --no-ff feature/a $ git push origin master
  ```

  

- 合并完成之后，确定该分支不再使用，则删除本地和远程上的该分支

  ```shell
  $ git branch -d feature/a $ git push origin --delete feature/a
  ```

  

## **4 测试新版本**

当新功能基本完成之后，我们要开始在release分支上测试新版本，在此分支上进行一些整合性的测试，并进行小bug的修复以及增加例如版本号的一些数据。版本号根据 master 分支当前最新的tag来确定即可，根据改动的情况选择要增加的位

- 开发布分支

  ```shell
  # 在feature分支中开 $ git checkout -b release/1.2.0 # 将分支推送到远程(如果有必要) 
  $ git push origin release/1.2.0:release/1.2.0
  ```

- 保证本地的release分支处于最新状态

  ```shell
  # 将本地的release分支内容更新为线上的分支 
  $ git pull origin release/1.0.0
  ```

  

- 制定版本号

  ```shell
  # commit 一個版本，commit的信息为版本升到1.2.0」 # git commit -a相当于git add . 再git commit $ git commit -a -m "Bumped version number to 1.2.0"
  ```

  

- 将已制定好版本号等其他数据和测试并修复完成了一些小bug的分支合并到主分支

  ```shell
  # 切换至主要分支 
  $ git checkout master 
  # 将release/1.2.0分支合并到主要分支 
  $ git merge --no-ff release/1.2.0 
  # 上tag 
  $ git tag -a "1.2.0" HEAD -m "新版本改动描述"
  ```

  

- 删除分支

  ```shell
  $ git branch -d release/1.2.0 $ git push origin --delete release/1.2.0
  ```

  

## **5 修补线上Bug**

- 此修复bug针对的是线上运行的版本出现了bug，急需短时间修复，无法等到下一次发布才修复，区别于**开发过程中develop上的bug，和测试过程中的release上的bug，这些bug，在原分支上改动便可以。**

- 在master根据具体的问题创建hotifix分支，并推送到远程

  ```shell
  git checkout master git checkout -b hotfix/typo git push origin hotfix/typo:hotfix/typo
  ```

 

- 制定版本号，一般最后位加1

  ```shell
  # commit 一個版本，commit的信息是版本跳 
  $ git commit -a -m "Bumped version number to 1.2.1"
  ```

  

- 修正后commit并将本地的hotfix分支更新为线上最新的版本

  ```shell
  $ git commit -m "..." 
  $ git pull origin hotfix/typo
  ```

  

- 将刚修复的分支合并到主分支

  ```
  # 切换到主要分支 
  $ git checkout master 
  # 將hotfix分支合并到主要分支 
  $ git merge --no-ff hotfix/typo 
  # 上tag 
  $ git tag -a "1.2.1" HEAD -m "fix typo"```

  ```
  
- 删除修补分支
