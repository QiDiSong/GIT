先写解决方案：

首先git reflog查看要回退的commit ID

然后git reset commitID 

切记不能用git reset --hard，这样会把本次修改的内容全部删除，就需要继续进行修改才可以提交了

# git commit的撤销方法：

某同事执行git commit 时太兴奋，执行了

```
git commit --amend
```

慌了，不敢编辑上一个commit的description了，直接选择了`wq`退出，然而git毕竟强大，默认将改动合并提交并覆盖了上一个commit生成了一个新的`commit id`，这下更慌了，上一个`commit id`在`git log`里没了，没了，没了
此时只有两个字，奔溃

### 好在git有撤销方法，下面的代码拿来举例。

------

当前代码仓有如下文件：

```
$ ll



total 4



-rw-r--r-- 1 ****** 1051817 3 九月 13 15:49 1.txt



-rw-r--r-- 1 ****** 1051817 3 九月 13 16:00 2.txt



-rw-r--r-- 1 ****** 1051817 2 九月 13 14:16 3.txt



-rw-r--r-- 1 ****** 1051817 7 九月 13 13:54 README.md
```

修改`1.txt`和`2.txt`，并提交。

```
$ echo "12" >> 1.txt



$ echo "22" >> 2.txt



$ git add .



$ git commit -m "modified 1/2.txt"



[master b82585f] modified 1/2.txt



 3 files changed, 3 insertions(+)



 create mode 100644 3.txt
```

在未push前，继续修改`3.txt`，并执行`git commit --amend`覆盖上一个commit。

```
$ echo "32" >> 3.txt



$ git add 3.txt



$ git commit --amend



[master 6889e84] modified 1/2/3.txt



 Date: Thu Sep 14 14:18:40 2017 +0800



 3 files changed, 4 insertions(+)



 create mode 100644 3.txt
```

执行`git log`发现最新的commit id 已经从`b82585f`变为了`6889e84`

```
$ git log



commit 6889e8402d8f40b38f5e6c0aff686b6b3ca82389 (HEAD -> master)



Author: ******



Date:   Thu Sep 14 14:18:40 2017 +0800



 



    modified 1/2/3.txt



 



$ git status



On branch master



Your branch is ahead of 'origin/master' by 1 commit.



  (use "git push" to publish your local commits)



nothing to commit, working tree clean,
```

这就演示到文章开头同事遇到的问题了，amend覆盖了上一个commit，且提交了对文件的修改，只是他未修改description。

------

### 撤销开始

如果我们忘记了被覆盖的那个commit id，那就用`reflog`命令看一下

```
$ git reflog



6889e84 (HEAD -> master) HEAD@{0}: commit (amend): modified 1/2/3.txt



b82585f HEAD@{1}: commit: modified 1/2.txt



 



$ git reset b82585f



Unstaged changes after reset:



M       3.txt


$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.

  (use "git push" to publish your local commits)

Changes not staged for commit:

  (use "git add <file>..." to update what will be committed)


  (use "git checkout -- <file>..." to discard changes in working directory)
  
        modified:   3.txt

no changes added to commit (use "git add" and/or "git commit -a")

$ git log

commit b82585f65b4c467a7e5855191b73a613fcf20892 (HEAD -> master)

Author: ares.wang <ares.wang@seraphic-corp.com>

Date:   Thu Sep 14 14:18:40 2017 +0800

    modified 1/2.txt 
```

可以看出，reset已经恢复到上一个被覆盖的`commit id`了，且对文件的修改回退到代码仓not staged的状态了

> 不使用 `git reset --hard`的目的就是为了保留本地修改，否则修改就会被丢弃！演示如下：

```
$ git reset --hard b82585f

HEAD is now at b82585f modified 1/2.txt


$ git status

On branch master


Your branch is ahead of 'origin/master' by 1 commit.



  (use "git push" to publish your local commits)



nothing to commit, working tree clean
```

3.txt的modified没有了，切记慎用`--hard`参数，除非你确定放弃当前未提交的所有修改！
