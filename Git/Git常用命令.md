## 一、基本命令

### 1、配置命令

#### 配置自己git的用户名

全局配置：`git config --global user.name "Your Name"`

当前仓库配置：`git config --local user.name "Your Name"`

#### 配置自己的邮箱

`git config --global user.email "email@example.com"`

#### 拉取、上传免密码

`git config --global credential.helper store`

#### 创建SSH Key

`ssh-keygen -t rsa -C "youremail@example.com"`

或 `ssh-keygen -t rsa -C "计算机名"`

#### 初始化仓库

初始化一个git仓库（版本库）

`git init`



### 2、查看命令

#### 查看用户的配置

`git config --global --list`

#### 查看系统的配置

`git config --system --list`

#### 查看当前仓库的配置

`git config --local --list` 或 `git config -l --local`

#### 查看工作区状态

`git status`

#### 查看修改内容

`git diff`

#### 查看提交历史

`git log`

#### 提交历史单行输出

`git log --pretty=oneline`

#### 查看命令历史

`git reflog`

#### 查看版本差异

查看工作区和版本库里面最新版本的区别

`git diff HEAD -- file`

#### 查看引用日志

`git reflog`



### 3、提交命令

#### 添加文件到仓库

（添加新文件时和修改文件时都用此命令）把文件修改添加到暂存区

`git add <file>`

#### 把文件提交到仓库

实际上就是把暂存区的所有内容提交到当前分支

`git commit -m <messages>`



### 4、回退命令

#### 回到上一个版本

`git reset --hard HEAD^` 

#### 回到指定版本

`git reset --hard commit_id`

#### 撤销暂存

把暂存区的修改撤销掉（unstage），重新放回工作区

`git reset HEAD <file>`

`git reset` 命令既可以回退版本，也可以把暂存区的修改回退到工作区。HEAD表示最新的版本

新命令：

`git rm --cached <file>`

`git restore --staged <file>...` 单文件

#### 丢弃工作区的修改

`git checkout -- file`

`git restore <file>...`

这里有三种情况

- 一种是文件自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态 
- 一种是文件已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态
- 一种是文件已经加入到了版本库，文件作了修改，现在撤销修改就回到加入到版本库后的状态

#### 恢复误删文件

把误删的文件恢复到最新版本

`git checkout -- file`

`git checkout` 其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”

#### 删除版本库中文件

`git rm <file>`

#### 取消文件被版本控制

`git rm -r --cached 文件/目录名`

`git update-index --assume-unchanged file`  忽略单个文件

`git update-index --no-assume-unchanged file`  取消忽略的文件

`git rm -r --cached .`  取消全部文件

#### 撤销创建仓库后第一次提交的commit

`git update-ref -d HEAD`



## 二、远程相关命令

### 1、先有本地库，后有远程库，关联远程库 

#### 关联远程库

`git remote add origin git@server-name:path/repo-name.git`

关联远程库之前必须先在远程库上建立一个空的库

#### 删除远程库关联

`git remote rm origin`

#### 推送本地库

把本地git仓库推送到远程git仓库（第一次推送本地到远程时要加-u）

`git push -u origin master`

#### 拉取远程库

`git pull`  默认拉取与本地分支对应的远程分支

`git pull origin master`

`git pull origin dev`

`git fetch`  获取远程仓库中所有分支到本地 

#### 查看远程库的信息

`git remote -v`

#### 分支关联

在本地创建和远程分支对应的分支，名称一致

`git checkout -b <branch-name> origin/<branch-name>`

建立本地分支和远程分支的关联

`git branch --set-upstream-to origin/<branch-name>`

例如：`git branch --set-upstream-to origin/master`



### 2、已经有远程库，从远程库克隆

#### 克隆仓库

`git clone git@server-name:path/repo-name.git`

#### 推送仓库

把本地master分支的最新修改推送至远程（不用加-u）

`git push origin master` 或 `git push`



## 三、分支相关命令

### 1、查看命令

#### 查看分支列表

`git branch`

#### 查看所有分支的最后一次操作

`git branch -v`

#### 查看所有分支的最后一次操作及远程分支名

`git branch -vv`

#### 查看分支合并图

`git log --graph`

`git log --graph --pretty=oneline --abbrev=commit`

#### 查看与当前分支合并过的其他分支

`git branch --merged`

#### 查看与未与当前分支合并过的其他分支

`git branch --no-merged`



### 2、删除命令

#### 删除分支

`git branch -d <name>`

#### 强行删除分支

`git branch -D <name>`

#### 删除远程分支

`git push origin :<branch-name>`

`git push origin --delete <branch-name>`

例如：删除远程 `master` 分支

`git push origin :master`

`git push origin --delete master`



### 3、操作命令

#### 创建分支

`git branch <name>`

#### 切换分支

`git checkout <name>`

创建分支并切换到创建的分支

`git checkout -b <name>`

#### 合并分支

合并某个分支到当前的分支

`git merge <name>`

使用普通模式合并分支，可以从分支历史上看出分支信息

`git merge --no-ff -m "合并描述" 分支名`

例如：`git merge --no-ff -m "merge wih no-ff" dev`

#### 整理分支

把本地未push的分叉提交历史整理成直线

`git rebase`



### 4、暂存命令

#### 暂存工作区

把现场工作“储藏”起来，等以后恢复现场后继续工作

`git stash`

#### 查看“储藏”的工作区

`git stash list`

#### 恢复储藏区

恢复后stash并不删除

`git stash apply`

#### 恢复指定的stash

`git stash apply stash@{0}`

#### 恢复储藏区同时删除stash

`git stash pop`

#### 删除stash

`git stash drop`

#### 删除某次stash

`git stash drop stash@{0}`



## 四、标签相关命令

### 1、创建标签

默认是在最新提交的commit上

`git tag <name>`

例如：`git tag v1.0`

指定某个commit打标签

`git tag v1.0 commit_id`

例如：`git tag v1.0 1094adb`

添加标签并增加备注

`git tag -a <tagname> -m <message>`



### 2、查看标签

查看所有标签

`git tag`

查看标签信息

`git show <tagname>`

例如：`git show v1.0`

`git tag -a v0.1 -m "version 0.1 released" commit_id`

(-a)指定标签名，(-m)指定说明文字

标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。



### 3、删除标签

删除本地标签

`git tag -d <tagname>`

删除远程标签（先删除本地）

`git push origin :refs/tags/标签名`



### 4、推送标签

推送标签到远程

`git push origin <tagname>`

一次性推送所有未推送到远程的本地标签

`git push origin --tags`



## 五、自定义Git

### 1、让git显示颜色

`git config --global color.ui true`



## 六、忽略特殊文件

### 1、配置文件

[在线配置文件]( https://github.com/github/gitignore )



### 2、忽略文件的原则

- 忽略操作系统自动生成的文件，比如缩略图等；忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件。
- 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。



### 3、相关命令

查看某个文件的忽略规则

`git check-ignore -v 文件名` 

例如：`git check-ignore -v app.class`

当文件被忽略时，添加不了，可以强制添加到git

`git add -f app.class`



## 七、配置别名

1. status起别名为st

   `git config --global alias.st status`

2. checkout 别名co

   `git config --global alias.co checkout`

3. commit 别名ci

   `git config --global alias.ci commit`

4. branch 别名br

   `git config --global alias.br branch`

5. reset HEAD别名为unstage

   `git config --global alias.unstage 'reset HEAD'` 

6. log -1别名为last (git last --显示最近的一次的提交)

   `git config --global alias.last 'log -1'`
   
   配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。
   
   每个仓库的Git配置文件都放在`.git/config`文件中。
   
   当前用户的Git配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中。



## 八、搭建Git服务器

[参考博客](https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664)



## 九、其他

### 1、解决中文乱码问题

`git config --global core.quotepath false`



### 2、解决换行符问题

windows出现 `warning:LF will be replaced by CRLF`的问题，[参考博客](https://blog.csdn.net/starry_night9280/article/details/53207928)

```shell
rm -rf .git // 删除.git
git config --global core.autocrlf false // 禁用自动转换
git init
git add .
```

core.autocrlf 的默认值是true

在团队中需要大家都统一配置，保持配置一致，不然会出现个人与团队不一致情况

