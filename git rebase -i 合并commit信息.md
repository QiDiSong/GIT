在使用 Git 作为版本控制的时候，我们可能会由于各种各样的原因提交了许多临时的 commit，而这些 commit 拼接起来才是完整的任务。那么我们为了避免太多的 commit 而造成版本控制的混乱，通常我们推荐将这些 commit 合并成一个。

**1,查看提交历史，git log**

首先你要知道自己想合并的是哪几个提交，可以使用git log命令来查看提交历史，假如最近4条历史如下：

```
commit 3ca6ec340edc66df13423f36f52919dfa3......

commit 1b4056686d1b494a5c86757f9eaed844......

commit 53f244ac8730d33b353bee3b24210b07......

commit 3a4226b4a0b6fa68783b07f1cee7b688.......
```

历史记录是按照时间排序的，时间近的排在前面。

**2,git rebase**

想要合并1-3条，有两个方法

1.从HEAD版本开始往过去数3个版本

```
git rebase -i HEAD~3
```

2.指名要合并的版本之前的版本号

```
git rebase -i 3a4226b
```

<u>**请注意3a4226b这个版本是不参与合并的，可以把它当做一个坐标**</u>

**3,选取要合并的提交**

1.执行了rebase命令之后，会弹出一个窗口，头几行如下：

```
pick 3ca6ec3   '注释**********'

pick 1b40566   '注释*********'

pick 53f244a   '注释**********'
```

2.将pick改为squash或者s,之后保存并关闭文本编辑窗口即可。改完之后文本内容如下：

```
pick 3ca6ec3   '注释**********'

s 1b40566   '注释*********'

s 53f244a   '注释**********'
```

3.然后保存退出，Git会压缩提交历史，如果有冲突，需要修改，修改的时候要注意，保留最新的历史，不然我们的修改就丢弃了。修改以后要记得敲下面的命令：

```
git add .  

git rebase --continue  
```

如果你想放弃这次压缩的话，执行以下命令：

```
git rebase --abort  
```

4.如果没有冲突，或者冲突已经解决，则会出现如下的编辑窗口：

```
# This is a combination of 4 commits.  
#The first commit’s message is:  
注释......
# The 2nd commit’s message is:  
注释......
# The 3rd commit’s message is:  
注释......
# Please enter the commit message for your changes. Lines starting # with ‘#’ will be ignored, and an empty message aborts the commit.
```

5.输入wq保存并推出, 再次输入git log查看 commit 历史信息，你会发现这两个 commit 已经合并了。
