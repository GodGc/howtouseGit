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






