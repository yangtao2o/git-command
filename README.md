# Git 常用命令

## 基本使用

- Workspace: 工作区
- Index / Stage: 暂存区
- Repository: 仓库区（或本地仓库）
- Remote: 远程仓库

![image](https://user-images.githubusercontent.com/19526072/49999253-4c690980-ffd1-11e8-892a-bff60b374d12.png)

### 下载

#### Windows

操作系统是 Windows，下载[git for windows](https://gitforwindows.org/)进行安装。

#### MacOS

操作系统是 Mac OS , 使用 brew 命令来安装。[git for macOS](https://git-scm.com/download/mac)

```sh
brew install git
```

### 配置

Git 的设置文件为 `.gitconfig` ，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

显示配置信息

```sh
git config --list
```

修改

```sh
git config --global user.name "yangtao"
git config --global user.email "xxx@.qq.com"
```

### 新建

在当前目录 git-command 下新建 Git 代码库，（会生成 .git 文件）

```sh
git init
```

新建目录 git-command 并将其初始化为 Git 代码库

```sh
git init git-command
```

从线上获取一个完整的项目代码

```sh
git clone https://github.com/yangtao2o/git-command.git
```

### 暂存、删除

添加指定文件到暂存区

```sh
git add index.html
```

添加指定目录到暂存区，包括子目录

```sh
git add assets
```

添加当前目录的所有文件到暂存区

```sh
git add .
```

添加每个变化前，都会要求确认，对于同一个文件的多处变化，可以实现分次提交

```sh
git add -p
```

删除工作区文件，并将这次删除加入暂存区

```sh
git rm [file1] [file2] ...
```

停止追踪指定文件，但该文件会保留在工作区

```sh
git rm --cached [file]
```

修改文件名，并放入暂存区

```sh
git mv index.html index-new.html
```

### 状态查看

运行 `git status` 检查存储库的当前状态

```sh
apple@dataozideiMac ~/Documents/tao/git-command
(master **)$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md
        deleted:    index-new.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        index-new2.html

no changes added to commit (use "git add" and/or "git commit -a")
```

返回结果内容：

1. 当前分支数据是 `origin/master`
1. `README.md` 文件已更改，`index-new.html`已删除，都尚未提交到仓库
1. 提示下一步操作。提交或删除`git add/rm <file>...`更改、或者撤销`git restore <file>...`更改
1. 未跟踪文件`index-new2.html`提交 `git add <file>`更改

### 撤销

`git restore <file>...`可使用暂存区数据覆盖工作区（working directory），即撤销掉本次修改

```sh
git restore index-new.html

 (master **)$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        index-new2.html

no changes added to commit (use "git add" and/or "git commit -a")
```

### 提交更改

暂存区提交到仓库区 ( -m (msg) )

```sh
git commit -m "My first commit"
```

指定文件提交

```sh
git commit [file1] [file2] ... -m [message]
```

提交工作区自上次 commit 之后的变化，直接到仓库区

```sh
git commit -a
```

提交时显示所有的 diff 信息

```sh
git commit -v
```

使用一次新的 commit ，提交上一次提交；如果代码没有任何变化，则用来改写上一次 commit 的提交信息

```sh
git commit --amend -m "new commit"
```

重做上一次 commit ，并包括指定文件的新变化

```sh
git commit --amend [file1] [file2] ...
```

### 分支

```sh
# 列出所有的本地分支
git branch

# 列出所有的远程分支 ( -r (remotes))
git branch -r

# 列出所有的本地分支和远程分支
git branch -a

# 新建一个分支，但依然停留在当前分支
git branch primary

# 新建，并切换至 该分支
git checkout -b primary-yt

# 新建，指向指定 commit
git branch [branch] [commitID]

# 新建，与指定的远程分支建立追踪关系
git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
git checkout [branch-name]

# 切换到上一个分支
git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
git branch --set-upstream [branch] [remote-branch]

# 合并指定分支 master-yt 到当前分支 master
git merge master-yt

# 选择一个 commit，合并进当前分支
git cherry-pick [commitid]

# 删除分支
git branch -d master-ytt

# 删除远程分支
git push origin --delete [branch-name]
git branch -dr [remote/branch]
```

### 标签

```sh
# 列出标签
git tag
```

### 查看信息

显示当前分支的版本历史

```sh
git log

# 单行展示
git log --pretty=oneline

# 列出最多两条提交纪录
git log --pretty=oneline --max-count=2

# 列出最近5分钟内的所有提交
git log --pretty=oneline --since='5 minutes ago'

# 列出5分钟之前的所有提交
git log --pretty=oneline --until='5 minutes ago'

# 列出指定作者的提交
git log --pretty=oneline --author=<your name>

# 列出所有分支的提交
git log --pretty=oneline --all

# 定制记录的显示格式
# --graph git 以 ASCII 图形布局的形式显示提交树
# --date=short 保持日期格式简短美观。
$ git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short
```

`git reflog`显示整个本地仓储的 commit, 包括所有 branch 的 commit，甚至包括已经撤销的 commit, 只要 HEAD 发生了变化, 就会在 reflog 里面看得到

比如回滚记录

```sh
22d2e82 (HEAD -> master, origin/master, origin/HEAD) HEAD@{0}: reset: moving to HEAD^
cafa257 HEAD@{1}: pull: Fast-forward
22d2e82 (HEAD -> master, origin/master, origin/HEAD) HEAD@{2}: reset: moving to HEAD^
cafa257 HEAD@{3}: commit: test: 回滚4
22d2e82 (HEAD -> master, origin/master, origin/HEAD) HEAD@{4}: reset: moving to HEAD^
25d1741 HEAD@{5}: commit: test: 回滚3
22d2e82 (HEAD -> master, origin/master, origin/HEAD) HEAD@{6}: reset: moving to HEAD^
2130aa8 HEAD@{7}: commit: test: 回滚2
22d2e82 (HEAD -> master, origin/master, origin/HEAD) HEAD@{8}: commit: test: 回滚
```

查看暂存区域（索引）内容

```sh
git ls-files -s
```

### 回滚

```sh
git reset [<mode>] [<commit>]
```

三种常用 mode 方式：mixed（默认值）、soft、hard 等

```sh
# reset 只会用 HEAD 移动节点，索引和工作目录内容未受影响
git reset --soft  HEAD^

# reset 会用 HEAD 指向的当前快照的内容来更新暂存区（索引）
git reset --mixed  HEAD^

# reset 继续这 --mixed 的操作覆盖索引，同时会将索引内容覆盖工作目录
git reset --hard  HEAD^
```

所以，回滚的等级也就出来了，soft 小于 mixed，mixed 又小于 hard，执行 hard 操作，一定要小心，它会真正地销毁数据。其他任何形式的 reset 调用都可以轻松撤消，但是 --hard 选项不能，因为它强制覆盖了工作目录中的文件。

## 参考目录

- [Git 常用命令](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)
- [Git 原理入门](http://www.ruanyifeng.com/blog/2018/10/git-internals.html) - 阮一峰
- [Git 从放弃到入门](https://juejin.cn/column/6969263852206686221) - 有图有真相，非常适用理解原理
- [Git 教程 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
- [珍藏多年的 Git 问题和操作清单](https://mp.weixin.qq.com/s/14WBS4GcZlEbBumfUagXMA)
- [Git 教程 - Maxsu](https://www.yiibai.com/git) --- 易百教程
