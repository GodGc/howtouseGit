## git-learn



### init:

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

`git status`命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，readme.txt被修改过了，但还没有准备提交的修改。

- 要随时掌握工作区的状态，使用`git status`命令。
- 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。



如何将更新后的文件 更新到 仓库中:

1. 先查看一下仓库状态:

   ```shell
   $ git status
   # 输出结果如下
   On branch master
   # readme.txt被修改过了，但还没有准备提交修改。
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git checkout -- <file>..." to discard changes in working directory)
   
       modified:   readme.txt
   
   no changes added to commit (use "git add" and/or "git commit -a")
   ```

2. 查看具体的修改内容:

   ```shell
   # diff 其实就是different的意思
   
   $ git diff readme.txt 
   diff --git a/readme.txt b/readme.txt
   index 46d49bf..9247db6 100644
   --- a/readme.txt
   +++ b/readme.txt
   @@ -1,2 +1,2 @@
   # 如下就是修改内容
   # -开头的一行是过去版本
   # +开头是修改后版本
   -Git is a version control system.
   +Git is a distributed version control system.
    Git is free software.
   ```

3. 提交修改:  

   - 添加到仓库

   ```shell
   git add readme.txt
   ```

   - 在提交修改之前可以看一下仓库的状态

   ```shell
   $ git status
   On branch master
   # 将要被提交的修改包括readme.txt
   Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)
   
       modified:   readme.txt
   ```

   - 提交修改:

   ```shell
   git commit -m "new introduction for this commit"
   [master e475afc] add distributed
    1 file changed, 1 insertion(+), 1 deletion(-)
   ```

4. 提交过后查看仓库状态

   ```shell
   git status
   On branch master
   # Git 告诉我们当前没有需要提交的修改，而且，工作目录是干净（working tree clean）的
   nothing to commit, working tree clean
   ```



### 版本回退

#### 查看版本日志

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

#### 回退版本

把当前版本`append GPL`回退到上一个版本`add distributed`，可以使用`git reset --hard commit_id`命令

```SHELL
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

> 可以继续 `git reset` 回退到上一个版本`wrote a readme file`



#### 如果前进到之前的新版本

**通过`git log`查看,发现回退后 最初的HEAD版本,也就是之前的最新版本不见了!**

解决办法:

1. **最简单的办法**:

   使用`git reset --hard`命令改变了HEAD指向某个版本后，还可以用`git log --reflog`查看所有版本。

2. **其他方法**

   只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个`append GPL`的`commit id`是`1094adb...`，于是就可以指定回到未来的某个版本：

   ```shell
   # 版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。
   git reset --hard 1094a
   ```

3. 而且:

   git提供了一个命令可以查看你曾经输入过的命令`git reflog`,其中带有**reset**字样就是曾经的回退命令,找寻那一行的commit_id就可再回退到某个版本,当然最好根据 commit提交时写的说明来回归

```shell
$ git reflog
ee06fa5 (HEAD -> master) HEAD@{0}: reset: moving to ee06f
1de5171 HEAD@{1}: reset: moving to HEAD^
ee06fa5 (HEAD -> master) HEAD@{2}: commit: change the list style for this note
1de5171 HEAD@{3}: commit: how to save changes to git-repository
47d69f6 HEAD@{4}: commit (initial): git-learn notes
```



#### 总结一下

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。





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

git管理有3个状态

1. modified 已修改

2. staged 已暂存

3. commit 已提交


从暂存区恢复文件到工作区

```shell
git checkout 文件名
```





#### 撤销修改

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退方法，不过前提是没有推送到远程库。



### 添加远程库

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联后，使用命令`git push -u origin master`第一次推送 master 分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；



### 创建分支

```shell
$ git checkout -b dev
Switched to a new branch 'dev'

# 这一句相当于2句的效果,创建 dev branch 然后切换到dev分支上
$ git branch dev
$ git checkout dev
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



### git分支管理模型

#### 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了



![gitflow](D:\Enotes\08-git\gitflow.jpg)



#### bug分支

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场。