[TOC]



# 分支

查看分支

```bash
git branch
```

创建分支，根据当前分支进行创建

```bash
git branch <branch-name>
```

切换分支

```bash
git checkout <branch-name>
git switch <branch-name> # 新的命令，比git checkout更容易理解
```

创建并切换分支

```bash
git checkout -b <branch-name>
# 相当于git branch <branch-name> + git checkout <branch-name>
```

删除本地分支

```bash
git branch -d <branch-name>
git branch -D <branch-name> # 强制删除，如果这个分支真的没有用

```

删除远程分支

```bash
git branch -d -r <branch-name>
```

合并分支，合并指定分支到当前分支

```bash
git merge <branch-name>
# 假设当前分支是master，git merge dev的意思是把dev分支合并到master
```

如果`git merge`失败，说明存在冲突，必须手动解决冲突后再合并。`Git`会直接在冲突的文件中标记出不同分支的内容，所以需要打开冲突的文件进行手动处理。

使用`git status` 可以告诉我们冲突的文件以及冲突的内容。

使用`git log —graph`可以查看分支合并图。

合并分支时，Git默认使用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。如果要强制禁用`Fast forward` 模式，Git就会在merge时生成一个新的commit，这样就可以从分支历史上看出分支信息。

把指定分支合并到当前分支，禁用`Fast forward`模式，需要使用`—no-ff`参数，以及`-m`参数及其对应的描述。

```bash
git merge --no-ff -m <msg> <branch-name>
```

## Bug分支

如果要修复一个bug，可以通过创建一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

如果接到一个修复bug的任务，但是想创建一个分支`issue-101`来修复它，但是当前在`dev`分支上进行的工作还没有完成，没法提交，怎么办？

使用`git stash`可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作

```bash
git stash
```

现在，使用`git status` 查看工作区，就是干净的，因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：

```bash
git checkout master # git switch master 

git checkout -n issue-101
```

修复bug，修改代码并提交：

```bash
git add xxx
git commit -m "fix bug 101" # 假设提交的记录为 [issue-101 4c805e2] fix bug 101
```

切换到`master`分支，进行合并，最后删除`issue-101`分支：

```bash
git checkout master
git merge --no-ff -m "merged bug fix 101" issue-101
```

回到`dev`分支，恢复原来的工作内容：

```bash
git checkout dev

git status

git stash list # 查看git stach的内容

git stash pop # 恢复最近的工作现场
# 等于 git stash apply <stach_name> + git stash drop <stash_name>
```

在`master`分支上修复了bug后，我们要想一想，`dev`分支是早期从`master`分支分出来的，所以，这个bug其实在当前`dev`分支上也存在。

如何在dev分支上修复同样的bug？重复操作一次，提交不就行了？

有更简单的方法！先切换到`dev`分支，然后使用`git cherry-pick`把`4c805e2 fix bug 101`这个提交所做的修改“复制”到`dev`分支。

```bash
git checkout dev

git cherry-pick 4c805e2
```

# 多人协作

## 推送分支

把本地的`master`分支推送到远程仓库`origin`：

```bash
git push origin master
```

如果要推送`dev`分支，

```bash
git push origin dev
```

如果在本地新建了一个分支，但是远程没有，也可以进行推送。推送完远程会出现同名分支。

```bash
git push --set-upstream origin <branch-name> // 其实可以省略--set-upstream
```

另外，并不是所有分支都需要推送：

-   `master`分支是主分支，因此要时刻与远程同步；
-   `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
-   `bug`分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
-   `feature`分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

在Git中，分支完全可以在本地自己藏着玩，不推送到远程，对其他人就是不可以见的。是否推送，视你的心情而定！

## 拉取分支

一般来说，为了创建一个Git项目，首先会在`Github`等代码托管网站创建一个仓库，然后再clone这个仓库到本地。另外，进入公司工作参与多人协作的项目，也需要先clone远程库。

从远程库clone时，默认情况下，只能拉取`master`分支，所以在本地只能看到`master`分支。

```bash
git clone XXX

git branch # 只能看到 * master
```

查看远程库：

```bash
git remote # origin表示远程仓库

git remote -v # 更详细
```

此时，本地的`master`分支默认跟踪（track）远程的`origin/HEAD`。本地分支跟踪远程分支的意思是指，以`master`分支为例，在本地分支进行的修改经过`git add`、`git commit` 、`git push`后会同步到远程被跟踪的分支`origin/HEAD`。

另外，也可以直接把远程仓库的其他分支拉取到本地，同时默认让本地分支跟踪远程分支，此时的本地分支名最好与远程分支名相同。

```bash
git checkout -b <branch-name> origin/<branch-name> # 本地分支 远程分支

```

为了避免打错字导致本地分支名和远程分支名不相同（实际上不会有什么问题，只是强迫症），推荐使用：

```bash
git checkout --track origin/<branch-name>
```

如果因为某些原因，本地分支没有跟踪远程分支，但是又希望设置跟踪。一种方法是直接删除本地的分支，然后使用上面的命令重新拉取。另一种方法是使用下面的命令：

```bash
git branch --set-upstream-to=origin/<branch-name> <branch-name>
```

一般来说，即使拉取了远程的分支，也不会直接在上面进行开发，而是创建自己的分支。比如：

```bash
# 拉取了远程的dev分支到本地后，此时处于dev分支，
git checkout -b XXX-dev
```

在自己的`XXX-dev`分支开发后，通常都会merge到本地`dev`分支，再同步到远程`origin/dev`分支：

```bash
git checkout dev

git merge XXX-dev // 自己的分支一般不会冲突，如果冲突，说明你的分支有些混乱

git push origin dev
```

但是，在多人协作中，其他人也会push到远程`origin/dev`分支，所以本地的`dev`分支可能没有远程新。所以，如果是这样，对于上面的第三步来说，推送分支会失败，Git会提示：

```bash
Your branch is behind 'origin/dev' by...
```

此时，需要先拉取远程分支：

```bash
git checkout <branch-name>

git pull --ff-only # 只尝试快进合并，如果不行则终止当前合并操作
```

如果拉取失败，即把远程分支快速合并到本地分支失败，意味着其他人提交的修改和你的修改产生了冲突，需要手动解决。解决办法和`git merge`解决冲突完全一样，此时Git会在冲突的文件中标记出产生冲突的不同分支的内容。解决冲突后，就可以正常commit和push了。

总结，多人协作的工作模式通常是这样：

1.  首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2.  如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3.  如果合并有冲突，则解决冲突，并在本地提交；
4.  没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。



# 命令

## git clean

删除没有被add的文件：

```bash
git clean -n # 显示 要被删除的文件
git clean -f # 删除 文件
git clean -df # 删除 文件和目录
```



# 资料

官方文档：https://git-scm.com/docs

廖雪峰的Git教程：https://www.liaoxuefeng.com/wiki/896043488029600

