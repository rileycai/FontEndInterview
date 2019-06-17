# git学习笔记

参考链接：[git 基础](https://git-scm.com/book/zh/v1/%E8%B5%B7%E6%AD%A5-Git-%E5%9F%BA%E7%A1%80)

## git 使用技巧
### 拉去某个分支代码并强制覆盖本地
```git
git checkout -b newBranch  //切换到某个分支
git branch --set-upstream-to=origin/newBranch newBranch  //将本地分支和远程分支绑定
git fetch --all
git reset --hard origin/newBranch
```
### 合并前面的几次提交
```git
git log  //选择要回退的版本
git reset --soft becf4bc188cc
git add .
git commit -m "合并前面的几次提交"
git push --force
```

### git diff
+ 此命令比较的是工作目录中当前文件和暂存区域快照之间的差异，也就是修改之后还没有暂存起来的变化内容。
+ 若要看已经暂存起来的文件和上次提交时的快照之间的差异，可以用 ***git diff --cached*** 命令。

### git commit -a -m 'added new benchmarks'
+ Git 提供了一个跳过使用暂存区域的方式，只要在提交的时候，给 git commit 加上 -a 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤。

### git rm
+ 要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。
+ 如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 **-f**（译注：即 force 的首字母），以防误删除文件后丢失修改的内容。
+ 我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。换句话说，仅是从跟踪清单中删除。比如一些大型日志文件或者一堆 .a 编译文件，不小心纳入仓库后，要移除跟踪但不删除文件，以便稍后在 .gitignore 文件中补上，用 --cached 选项即可：***git rm --cached readme.txt***

### git log --stat
+ git log 会按提交时间列出所有的更新，最近的更新排在最上面。
+ ***--stat***，仅显示简要的增改行数统计

### git commit --amend
+ 有时候我们提交完了才发现漏掉了几个文件没有加，或者提交信息写错了。想要撤消刚才的提交操作，可以使用 --amend 选项重新提交：

### git remote -v
+ 要查看当前配置有哪些远程仓库，可以用 git remote 命令，它会列出每个远程库的简短名字。在克隆完某个项目后，至少可以看到一个名为 origin 的远程库，Git 默认使用这个名字来标识你所克隆的原始仓库。

### git checkout -b newbranch
+ 这里表示分支的创建与切换。

### git merge
+ 它会把两个分支最新的快照（C3 和 C4）以及二者最新的共同祖先（C2）进行三方合并，合并的结果是产生一个新的提交对象（C5）。

### git rebase master

```
git checkout experiment
git rebase master
```
+ 它的原理是回到两个分支最近的共同祖先，根据当前分支（也就是要进行变基的分支 experiment）后续的历次提交对象（这里只有一个 C3），生成一系列文件补丁，然后以基底分支（也就是主干分支 master）最后一个提交对象（C4）为新的出发点，逐个应用之前准备好的补丁文件，最后会生成一个新的合并提交对象（C3'），从而改写 experiment 的提交历史，使它成为 master 分支的直接下游

### git stash
+ 现在你想切换分支，但是你还不想提交你正在进行中的工作；所以你储藏这些变更。为了往堆栈推送一个新的储藏，只要运行 **git stash**。
+ 这时，你可以方便地切换到其他分支工作；你的变更都保存在栈上。要查看现有的储藏，你可以使用 **git stash list**。
+ 对文件的变更被重新应用，但是被暂存的文件没有重新被暂存。想那样的话，你必须在运行 ***git stash apply*** 命令时带上一个 --index 的选项来告诉命令重新应用被暂存的变更。
+ apply 选项只尝试应用储藏的工作——储藏的内容仍然在栈上。要移除它，你可以运行 ***git stash drop***，加上你希望移除的储藏的名字。
+ 你也可以运行 ***git stash pop*** 来重新应用储藏，同时立刻将其从堆栈中移走。

### git stash branch newbranch
+ 如果你想用更方便的方法来重新检验你储藏的变更，你可以运行 git stash branch，这会创建一个新的分支，检出你储藏工作时的所处的提交，重新应用你的工作，如果成功，将会丢弃储藏。

