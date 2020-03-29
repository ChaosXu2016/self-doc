# git相关笔记

## 手撕Git，告别盲目记忆---读书笔记

[原文地址](https://zhuanlan.zhihu.com/p/98679880)

### git分区

git在本地分为3个区：工作区，暂存区和版本库。

```sequence
工作区->暂存区:git add
暂存区->版本库:git commit
```

1. 工作区：就是我们平时工作的区域
2. 暂存区：我们会使用`git add`命令来将工作区的改动存放到暂存区，这部分存放在`.git/index`文件中
3. 版本库：这里是我们本地的`git`仓库，我们使用`git commit -m 'commit message'`来将我们的暂存区的改动提交到版本库，然后我们使用`git push`命令来讲本地版本库的改动推送到远程版本库，或者使用`git pull`命令来同步远程仓库到本地。

我们可以使用`git diff [head] [--cache]`来对比各个分区的改动：

1. 这里的`head`是提交的`commitId`，我们可以通过`git log`来查看某个提交记录的`id`，这样就可以对比当前工作区和任意一个历史版本的代码的改动情况。这里如果不加`head`则对比的是暂存区的文件，如果暂存区没有文件，则对比最近一次提交

2. `--cache`是表示对比的是暂存区还是工作区的文件改动，如果不加则表示对比的是工作区的改动。



### git原理

1. git的文件改动都存放在`.git/objects/`目录
2. git中文件类型分为3种，`commit|blob|tree`，其中`commit`记录每次提交信息，`blob`记录的是文件内容，`tree`记录的是文件目录。这些文件名都是各个内容的hash指纹。

### git分支

1. refs目录就是用来记录当前对分支的引用信息，包括**本地分支，远程分支，标签**。
2. heads记录的是本地所有分支，remotes和HEAD一样，指向**对应的某个远程分支**。
3. 目录中的文件名就是对应的分支名，文件内容中的hash值对应的是该分支的最新的commit的hash
4. **HEAD**就是表示当前在哪个分支
5. 我们可以用`git branch branchName`来创建分支，通过`git checkout branchName`来切换分支
6. 也可以使用`git checkout -b branchName`来创建并切换分支
7. 创建分支的时候我们通过手动输入head，可以创建从某个版本创建分支
8. 可以通过`git branch -v`来查看分支信息，`git branch -D branchName`来删除本地分支

