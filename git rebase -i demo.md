> 在使用 Git 作为版本控制的时候，我们可能会由于各种各样的原因提交了许多临时的 commit，而这些 commit 拼接起来才是完整的任务。那么我们为了避免太多的 commit 而造成版本控制的混乱，通常我们推荐将这些 commit 合并成一个。

首先假设我们有3个 commit

![img](https://upload-images.jianshu.io/upload_images/228805-ffd461efeb8a26a2.png?imageMogr2/auto-orient/strip|imageView2/2/w/1184/format/webp)

git-log-origin.png

我们需要将 `2dfbc7e8` 和 `c4e858b5` 合并成一个 commit，那么我们输入如下命令

![img](https://upload-images.jianshu.io/upload_images/228805-e96334b872909dc4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1184/format/webp)

git-rebase-i.png

其中，`-i` 的参数是不需要合并的 commit 的 hash 值，这里指的是第一条 commit， 接着我们就进入到 `vi` 的编辑模式

![img](https://upload-images.jianshu.io/upload_images/228805-fce11005e6ee8e2d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1184/format/webp)

git-rebase-edit.png

可以看到其中分为两个部分，上方未注释的部分是填写要执行的指令，而下方注释的部分则是指令的提示说明。指令部分中由前方的命令名称、commit hash 和 commit message 组成。

当前我们只要知道 `pick` 和 `squash` 这两个命令即可。

- `pick` 的意思是要会执行这个 commit
- `squash` 的意思是这个 commit 会被合并到前一个commit

我们将 `c4e858b5` 这个 commit 前方的命令改成 `squash` 或 `s`，然后输入`:wq`以保存并退出

![img](https://upload-images.jianshu.io/upload_images/228805-8c742e137feb7ce5.png?imageMogr2/auto-orient/strip|imageView2/2/w/1184/format/webp)

git-rebase-squash.png

这是我们会看到 commit message 的编辑界面

![img](https://upload-images.jianshu.io/upload_images/228805-e0ca7e9d694e3f52.png?imageMogr2/auto-orient/strip|imageView2/2/w/1184/format/webp)

git-rebase-commit-message.png

其中, 非注释部分就是两次的 commit message, 你要做的就是将这两个修改成新的 commit message。

![img](https://upload-images.jianshu.io/upload_images/228805-7bb2a59daa90975c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

git-rebase-commit-message-combination.png

输入`wq`保存并推出, 再次输入`git log`查看 commit 历史信息，你会发现这两个 commit 已经合并了。

![img](https://upload-images.jianshu.io/upload_images/228805-aa15aa1014eadbcf.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

git-rebase-log-new.png

**注意事项**：如果这个过程中有操作错误，可以使用 `git rebase --abort`来撤销修改，回到没有开始操作合并之前的状态。
