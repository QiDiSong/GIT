### 1. 本地修改了一些文件 (并没有使用 git add 到暂存区)，想放弃修改

- **单个文件/文件夹**：

  ```shell
   git checkout -- filename
  ```

- 所有文件/文件夹：

  ```shell
  git checkout .
  ```

### 2. 本地新增了一些文件 (<u>并没有 git add 到暂存区</u>)，想放弃修改

- 单个文件/文件夹：

  ```shell
  rm  -rf filename
  ```

- 所有文件：

  ```shell
  git clean -xdf
  ```

  > **删除新增的文件，如果文件已经已经 git add 到暂存区，并不会删除！**

- 所有文件和文件夹：

  ```shell
  git clean -xdff
  ```

  > [谨慎操作] 本命令删除新增的文件和文件夹，如果文件已经已经 git add 到暂存区，并不会删除！

### 3. 本地修改/新增了一些文件，<u>已经 git add 到暂存区</u>，想放弃修改

- 单个文件/文件夹：

  ```shell
  git reset HEAD filename
  ```

- 所有文件/文件夹：

  ```shell
  git reset HEAD .
  ```

### 4. 本地通过 git add 和 git commit 后，想要撤销此次 commit

- 撤销 commit, 同时保留该 commit 修改：

  ```shell
  git reset commit_id
  ```

  这个 `commit_id` 是你想要回到的那个节点，可以通过 git log 查看，可以只选前 6 位。

  > 撤销之后，你所做的已经 commit 的修改还在工作区！

- 撤销 commit, 同时本地删除该 commit 修改：

  ```shell
  git reset --hard commit_id
  ```

  这个 `commit_id` 是你想要回到的那个节点，可以通过 git log 查看，可以只选前6位

  > [谨慎操作] 撤销之后，你所做的已经 commit 的修改将会清除，仍在工作区/暂存区的代码也将会清除！

