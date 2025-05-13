# Git

### 常用指令

`git init` 创建git文件并关联

`git config --global user.name "[name]"`

`git config --global user.email "[email address]"`

设置提交代码时的用户信息，包括用户名和电子邮件地址

`git --version` 查看git版本

#### 常用

* `git add .` 将修改的文件添加到暂存区(.表示所有，如需明确具体文件换成路径即可)

* `git commit -m "版本注释"` 将暂存区的文件提交到本地仓库，-m表示加上注释

* `git status` 显示当前工作目录下文件的状态，包括已修改、已提交等

* `git log`  查看提交日志，包括提交信息、时间等

* `git reset --hard [commitId]` **回退到指定版本，功能强大**

`git remote -v` 查看关联的仓库

`git push -u origin master` “origin”是你给远程仓库起的别名，“master”你的主分支名称。`-u`参数表示设置上游分支，以后可以直接使用`git push`命令推送改动

#### 分支

* `git branch` 列出所有本地分支、远程分支或所有分支

* * `[新分支]` 创建一个新的分支
  *   `-d`  删除分支，但需要检查
  * `-D`  强制删除，不需要检查

* `git checkout [分支名]`  切换到指定分支，或创建一个新分支并切换到该分支
  * `-b [新分支]` 创建并切换到新分支
* `git merge [分支名]` 将指定分支的修改合并到当前分支

#### 远程仓库

* `git remote` 显示所有远程仓库的信息

  * `git remote add origin 地址` 添加远程仓库，origin为仓库名
  * `git remote -v` 更详细的显示远程仓库的信息

* `git push [-f] [--set-upstream] [远端名称[本地分支名][:远端分支名]]`
  将分支推送到远程仓库，如果远程分支名和本地分支名称相同,则可以只写本地分支

  * `--set-upstream` 推送到远端的同时并且建立起和远端分支的关联关系。

  * `git push --set-upstream origin master`

    如果当前分支已经和远端分支关联，则可以省略分支名和远端名。

    * `git push` 将master分支推送到已关联的远端分支。

* `git push` 将本地的修改推送到远程仓库。可以使用`--force`选项强行推送，使用`--all`选项推送所有分支，使用`--tags`选项推送所有标签

* `git clone` 克隆远程仓库到本地，可以指定要克隆的分支

* `git fetch [remote]` 下载远程仓库的所有变动，但不会自动合并
* `git merge [origin/name]` 合并远程分支到本地

* `git pull [remote] [branch]` 取回远程仓库的变化，并与本地分支合并。

#### 

#### 其他

`git diff` 比较文件或分支之间的差异

`git reflog` 查看仓库的操作历史，包括所有的commit、reset等操作

`git stash` 保存当前工作目录的修改，可以在需要的时候再恢复

`git tag` 列出所有标签，或创建一个新的标签在当前提交或指定提交上

* `cat [文件]` 查看文件内容

* `touch` 在当前目录下*创建文件*
* `vi  [file]` *修改文件内容*
* ![分支使用规范](C:\Users\J\AppData\Roaming\Typora\typora-user-images\image-20250119132053396.png)



#### 