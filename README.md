## Learn how to use git

### Git配置

```shell
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```



### 初始化一个本地仓库:

```shell
git init
```



### 添加文件到仓库:

1. 添加到暂存区:

```shell
git add readme.txt
```

2. 从暂存区提交到仓库:

```shell
git commit -m "this a introduction for this commit"
```



### 查看仓库修改状态

```shell
git status
```



### 查看修改内容

```shell
git diff
```



### 查看提交日志

```shell
git log
```

### 查看命令历史

```shell
git reflog
```





### 查看版本日志

使用` git log`命令显示从最近到最远的提交日志，我们可以看到3次提交,也就是3个版本,第一个HEAD是当前版本,上一个版本是HEAD^,上上个版本是HEAD^^

```shell
$ git log
# 一大串类似1094adb...的是commit id（版本号）
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

### 回退版本

```SHELL
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

> 以上命令是返回上一个版本，在Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本是`HEAD^^`，往上100个版本写成`HEAD~100`。



### 回退到指定版本

```shell
git reset --hard <commit-id> 
# commit-id 不必写全,git会
```







### 版本库（Repository）

把文件往Git版本库里添加的时候，是分两步执行的：

​	第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

​	第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

#### 工作区（Working Directory）

创建的git目录就是一个**工作区**,不包括其中`.git`目录,这是git自动创建的,这个不算工作区，而是Git的版本库。

#### 暂存区

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。



### git管理的3个状态

1. modified 已修改

2. staged 已暂存

3. commit 已提交


从暂存区恢复文件到工作区

```shell
git checkout 文件名
```





### 撤销修改
#### 丢弃工作区的修改

```shell
git checkout -- file
```

该命令是指将文件在工作区的修改全部撤销，这里有两种情况：

1. 一种是file自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
2. 一种是file已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次git commit或git add时的状态。

#### 丢弃暂存区的修改

1. 撤销掉暂存区的修改，重新放回工作区

```shell
git reset HEAD <file>
```

2. 撤销工作区的修改

```shell
git checkout -- file
```

### 场景3

已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退方法，不过前提是没有推送到远程库。



### 删除文件

```shell
git rm <file>
```

### 从版本库里面删除文件

1. 先从工作区删除
2. 然后提交版本到版本库

```shell
git commit -m "delete text.txt"
```



### 添加远程库

#### 创建SSH Key

```shell
ssh-keygen -t rsa -C "youremail@example.com"
```

#### 关联一个远程库

```shell
git remote add origin git@server-name:path/repo-name.git
```

#### 将本地的master分支推送到origin主机

```shell
git push -u origin master
```

> `-u` 表示第一次推送master分支的所有内容
>
> 同时指定origin为默认主机，后面就可以不加任何参数使用git push了。
>
> 不带任何参数的git push，默认只推送当前分支，这叫做simple方式。此外，还有一种matching方式，会推送所有有对应的远程分支的本地分支。Git 2.0版本之前，默认采用matching方法，现在改为默认采用simple方式。

#### 指定默认主机后, 从本地推送当前分支到远程库主分支

```
$ git push
```

如果推送失败，先用git pull抓取远程的新提交；

**从远程库抓取分支**

```
$ git pull
```

如果有冲突，要先处理冲突。

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；



### 克隆远程库

```shell
git clone https://github.com/username/repositoryname.git
```



### 创建分支

```shell
$ git checkout -b dev


# 这一句相当于2句的效果,创建 dev branch 然后切换到dev分支上
$ git branch dev
$ git checkout dev
```

### 查看分支

```shell
git branch
```

>  `git branch`命令会列出所有分支，当前分支前面会标一个*号。

### 切换分支

```shell
git checkout <branchname>
```

### 合并分支

`git merge`命令用于合并指定分支到当前分支。合并后，再查看 readme.txt 的内容，就可以看到，和`dev`分支的最新提交是完全一样的。




Git 鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建 + 切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`



### 合并 冲突

有时候使用git合并2个分支上的内容的时候,git没有办法自动合并内容,因为2个分支上的文件有冲突,那么我们就需要手动去修改了.

使用`git status` 查看冲突文件,然后打开该文件手动修改,保存后再提交到当前分支的仓库 add ... commit...

```shell
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

1.经过自己的测试，在合并出现问题之后，master分支的装填是merging,代表正在合并过程中
2.此时，两个文件已经是一个完整体了，虽然还是有冲突内容，修改完冲突内容之后，提交，此时两个文件已经合并成功了



### 查看分支合并图

```shell
git log --graph
```





### 删除分支

```shell
git branch -d <branchname>
```



### 保存工作现场

```shell
git stash
```

### 查看工作现场

```shell
$ git stash list
```

### 恢复工作现场

```shell
git stash pop
```



-----

### git分支管理模型


#### 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了

![gitflow](https://github.com/GodGc/howtouseGit/blob/master/gitflow.jpg)


#### bug分支

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场。


#### 丢弃没有被合并过的分支

如果要丢弃没有被合并过的分支，可以通过`git branch -D <name>`强行删除。



### 处理push远程仓库分支时的冲突

在多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`拉取远程分支的代码,会自动试图和本地代码进行合并；
3. 如果合并有冲突，则解决冲突，**并在本地提交**；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。



### 给分支上的commit打标签

发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。



**查看标签**

```shell
git tag
```

**新建标签**     --默认是在当前分支的HEAD

```shell
git tag <tagname>
```

**为指定commit新建标签**

```shell
git tag <tagname> <commit_id>
```

**指定标签信息**

```shell
git tag -a <tagname> -m "blablabla..."
```

**推送某个标签到远程，使用命令`git push origin <tagname>`：**

```shell
$ git push origin v1.0
Total 0 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
 * [new tag]         v1.0 -> v1.0
```

**一次性推送全部尚未推送到远程的本地标签：**

```shell
$ git push origin --tags
```

**删除本地标签:**

```shell
$ git tag -d v0.1
```

**删除远程标签:**

1. 先删除本地标签

   ```shell
   git tag -d v0.1
   ```

2. 远程删除。删除命令也是push，但是格式如下：

   ```shell
   $ git push origin :refs/tags/v0.1
   To github.com:michaelliao/learngit.git
    - [deleted]         v0.1
   ```



### 在github上参与开源项目

1. 在fork这个项目到自己的账号 -- 自己拥有Fork后的仓库的读写权限；
2. 本地克隆自己账号上的项目地址
3. 本地修改后发push到自己的github远程仓库
4. 如果希望这个开源项目的官方库能接受你的修改，可以在GitHub上发起一个pull request。当然，对方是否接受你的pull request就不一定了。



> 参考且十分感谢:[廖雪峰-git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)