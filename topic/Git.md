# Git 面试题

### 1. rebase 与 merge的区别?
1. rebase会把你当前分支的 commit 放到公共分支的最后面,所以叫变基。就好像你从公共分支又重新拉出来这个分支一样。
2. merge 会把公共分支和你当前的commit 合并在一起，形成一个新的 commit 提交.

### 2. git reset、git revert 和 git checkout 有什么区别?
+ **从 commit 层面来说：** 
1. git reset 可以将一个分支的末端指向之前的一个 commit。然后再下次 git 执行垃圾回收的时候，会把这个 commit 之后的 commit 都扔掉。
2. git checkout 可以将 HEAD 移到一个新的分支，并更新工作目录。因为可能会覆盖本地的修改，所以执行这个指令之前，你需要 stash 或者 commit 暂存区和工作区的更改。
3. git revert 和 git reset 的目的是一样的，但是做法不同，它会以创建新的 commit 的方式来撤销 commit，这样能保留之前的 commit 历史，比较安全。另外，同样因为可能会覆盖本地的修改，所以执行这个指令之前，你需要 stash 或者 commit 暂存区和工作区的更改。
+ **从文件层面来说**
1. git reset 只是把文件从历史记录区拿到暂存区，不影响工作区的内容。
2. git checkout 则是把文件从暂存区拿到工作区，不影响历史记录的内容。
3. git revert 不支持文件层面的操作。

![git](git-reset.png)
