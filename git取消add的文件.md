> 前言：在开发中，有时已经 add 的代码，想要取消恢复更改的文件，那么怎么办呢？

1、git checkout — //未git add的文件

2、git reset HEAD //已经git add的文件，可以用这个取消add，然后用上一条命令恢复

3、git reset –hard HEAD //把全部更改的文件都恢复（小心使用，不然辛辛苦苦写的全没了）

------

> git reset HEAD 可以全部恢复未提交状态

