#### GIT

1）、cd : 改变目录。
2）、cd . . 回退到上一个目录，直接cd进入默认目录
3）、pwd : 显示当前所在的目录路径。
4）、ls(ll): 都是列出当前目录中的所有文件，只不过ll(两个ll)列出的内容更为详细。
5）、touch : 新建一个文件 如 touch index.js 就会在当前目录下新建一个index.js文件。
6）、rm: 删除一个文件, rm index.js 就会把index.js文件删除。
7）、mkdir: 新建一个目录,就是新建一个文件夹。
8）、rm -r : 删除一个文件夹, rm -r src 删除src目录， 好像不能用通配符。
9）、mv 移动文件, mv index.html src index.html 是我们要移动的文件, src 是目标文件夹,当然, 这样写,必须保证文件和目标文件夹在同一目录下。
10）、reset 重新初始化终端/清屏。
11）、clear 清屏。
12）、history 查看命令历史。
13）、help 帮助。
14）、exit 退出。
15）、#表示注释

#### 创建版本库

$ git init

#### 放入Git仓库

1、git add 添加到仓库

2、git commit -m “  ” 提交到仓库，-m后是提交的说明

#### 工作区状态

git status，

#### 提交日志

git log  后可加参数 --pretty=oneline，一行显示

#### 版本回退

git reset --hard HEAD^回退到上次提交的版本

git reset --hard  commit_id 回退到commit_id版本号的版本

git reflog 记录了每一次命令

#### 撤销修改

git checkout -- readme.txt

1、修改后未放入暂存区，撤销修改回到和版本库一样的状态

2、已添加到暂存区，又做了修改，撤销修改回到添加到暂存区后的状态

git reset HEAD  readme.txt

命令git reset既可以回退版本，也可以把暂存区的修改退回到工作区。用HEAD表示最新的版本

#### 删除文件

rm tset.txt 删除工作区的test.txt文件

1、确实要从版本库中删除该文件

​	git rm test.txt

​	git commit -m""

2、删错了

​	git checkout -- test.txt

#### 远程库

关联一个远程库

​	git remote add origin git@github.com:dianyuan-chen/TestGit.git

​	origin是远程库的默认名字

第一次把本地库所有内容推送至远程库

​	git push -u origin master

把本地master分支的最新更新推送至GitHub

​	git push origin master

删除远程库

​	git remote rm origin

#### 从远程库克隆

git clone git@github.com:dianyuan-chen/TestGit.git

#### 分支

创建分支并切换到分支

​	git checkout -b dev/ git switch -c dev

=  git branch dev

​	git checkout dev/ git switch master

查看当前分支

​	git branch

合并分支

git merge dev

删除分支

git branch -d dev

#### 解决分支冲突

分支和master提交了不同的内容，在master|MERGING中修改文件后提交，即可解决冲突

git log --graph --pretty=oneline --abbrev-commit 查看合并情况

删除分支 git branch -d feature

强行删除 git branch -D feature

#### 分支管理策略

禁止Fast forward

git merge --no-ff -m "merge with no-ff" dev，会创建一个新的commit，（fast forward 只是将merge指针指向dev的提交）

一般不在直接master工作，在dev上工作，每个人有自己的分支，往dev上合并

#### BUG分支

代码写到一半，有bug需要修复，

1.git stash 把当场工作现场储存，恢复后继续工作，git stash list查看储藏的内容

2.从master创建一个临时分支 git checkout -b issue-101

3.复完成后，合并分支，最后删除issue-101分支

4.恢复储藏的分支 git stash apply恢复后需手动git stash drop删除

​	git stash pop恢复并删除

5.修复储藏分支的bug，把4c805e2 fix bug 101这个提交复制到dev分支git cherry-pick 4c805e2

#### 多人协作

git remote 查看远程库的信息 -v显示更详细的信息

推送分支：git push origin master

从远程库clone只能看待master分支，用git checkout -b dev origin /dev创建远程origin的dev分支到本地。

党对相同的文件做了修改，你再次试图推送，则推送失败，需把最新的提交从origin/dev抓下来：git pull，若pull失败，因为没有指定本地dev分支与远程origin/dev分支的链接，需设置链接：git branch --set-upstream-to=origin/dev dev,再pull：hit pull，后手动解决冲突

#### rebase

可以把本地未push的分叉提交历史 整理成直线

#### 常用术语

##### 仓库 Repository

收版本控制的所有文件修订历史的共享数据库

##### 工作空间 Workspace

本地硬盘上编辑的文件副本

##### 工作区 Working tree

工作区包含了仓库的工作文件。可以修改的内容和提交更改作为新的提交到仓库

##### 暂存区

是工作区用来提交更改（commit）前可以暂存工作区的变化

![](D:\CDY\笔记\img\git.png)

#### 文件的4种状态

##### Untracked 

未跟踪 此文件在文件夹中，但并没有加入到git库，不参与版本控制。通过git add状态改变位Staged

##### Unmodify 

文件已经入库，未修改。两种去除 1、被修改变为Modified 2、git rm移除版本库，成为Untracked

##### Modified 

文件已修改。两个去处 1、git add 进入暂存staged状态，使用git checkout丢弃修改，返回到unmodify状态，git checkout即从库中取出文件，覆盖当前修改

##### Staged

暂存状态 1、执行git commit将修改同步到库中，这是库中的文件和本地文件有变为一致，文件变为Unmodify状态 2、执行git reset HEAD filename取消暂存，文件状态为Modified。