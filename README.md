## git-learn



### init:

```shell
git init
```



### 添加文件到仓库并提交:

1. 添加到仓库:

```shell
git add readme.txt
```

2. 提交到仓库:

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

- 查看版本日志

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

- 回退版本

把当前版本`append GPL`回退到上一个版本`add distributed`，可以使用`git reset --hard commit_id`命令

```SHELL
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

> 可以继续 `git reset` 回退到上一个版本`wrote a readme file`



**但是通过`git log`查看,发现回退后 最初的HEAD版本,也就是之前的最新版本不见了!**

解决办法:

只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个`append GPL`的`commit id`是`1094adb...`，于是就可以指定回到未来的某个版本：

```shell
# 版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。
git reset --hard 1094a
```

而且:

git提供了一个命令可以查看你曾经输入过的命令`git reflog`,其中带有**reset**字样就是曾经的回退命令,找寻那一行的commit_id就可再回退到某个版本,当然最好根据 commit提交时写的说明来回归

```shell
$ git reflog
ee06fa5 (HEAD -> master) HEAD@{0}: reset: moving to ee06f
1de5171 HEAD@{1}: reset: moving to HEAD^
ee06fa5 (HEAD -> master) HEAD@{2}: commit: change the list style for this note
1de5171 HEAD@{3}: commit: how to save changes to git-repository
47d69f6 HEAD@{4}: commit (initial): git-learn notes
```

##### 现在总结一下：

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。
- 使用git reset --hard命令改变了HEAD指向某个版本后后，还可以用git log --reflog查看所有版本。


