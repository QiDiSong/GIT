当git add出现rewrite foo.bar(64%)的时候，这个文件已经被重写了，再上库的话会出问题<br>
这个时候，可以
1. git checkout -b branchName_backup 新建个本分支的备份，用于后面的git cherry-pick
2. git reset --hard cimmitID 回到修改前的版本，将这个文件拷贝出来，放到某路径下
3. git cherry-pick branchName_backup_commitID 将当时切出去分支所做的修改，重新拷贝至当前分支
4. cp ~/file 当时重复的那个file 将rewrite的那个文件还原回去
5. git push origin branchName 重新推上库
