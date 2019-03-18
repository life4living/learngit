[TOC]
# 目录初始化
## 目录git初始化
```bash
git init
```

## 取消git初始化, 在git目录下执行
```bash
rm -rf .git
```

# 创建版本库,提交修改
## 提交文件

第一步是用`git add` 把文件添加进去，实际上就是把文件修改添加到暂存区；


第二步是用 `git commit` | `git commit -m "<msg/comment>"` 提交更改，实际上就是把暂存区的所有内容提交到当前分支。


## 查看当前仓库状态
`git status` 命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，`readme.txt` 被修改过了，但还没有准备提交的修改。

## 查看修改情况
`git diff readme.txt ` (尚未commit前)



# 时光穿梭

## 版本回退
每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为commit。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个commit恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

### 查看历史记录
`git log` 命令查看历史记录
![git log](https://d.pr/i/ntxdjW+)


`git log --pretty=oneline`
![git log --pretty=oneline](https://d.pr/i/A1rI64+)
1. SHA1 计算版本号 `commit id`
2. `HEAD` 表示当前版本
3. 上个版本 `HEAD^`, 上上版本 `HEAD^^`, `HEAD~100`


### 回退版本
`git reset --hard HEAD^` 或 `git reset --hard <commit id>`
这里ID没必要全写, 前几位即可.

### 记录每一条命令(找回版本号)
`git reflog`
![](https://d.pr/i/E10f4F+)

## 工作区和暂存区

工作区隐藏目录 `.git` 为Git版本库
<center>
![版本库结构](https://d.pr/i/Dx0X0N+)<br>
版本库结构
</center>
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支 `master` ，以及指向 `master` 的一个指针叫 `HEAD` 。

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add` 把文件添加进去，实际上就是把文件修改添加到**暂存区**；

第二步是用 `git commit` 提交更改，**实际上就是把暂存区的所有内容提交到当前分支。**

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master` 分支，所以，现在，`git commit` 就是往 `master` 分支上提交更改。

`git add` <file> 将文件修改添加到暂存区,`git add` 后无法使用`git diff` 查看修改; `git commit` 将暂存区**所有内容**提交到当前分支(默认为master), `git commit` 后, 暂存区清空

## 管理修改
Git管理的是修改，当使用`git add` 命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit` 只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。

## 撤销修改
命令`git checkout -- readme.txt` 意思就是，把 `readme.txt` 文件在工作区的修改全部撤销，这里有两种情况：
1. `readme.txt` 自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
2. `readme.txt` 已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次`git commit` 或 `git add` 时的状态。

## 删除文件
`git rm <file>` `git commit` 从git版本库中删除
`rm <file> ` 仅本地工作目录删除, `git status` 查看会返回文件缺失, 版本库中文件没有改变, 可以使用 `git checkout <file>` 从版本库中回复已经删除的文件.

`git checkout` 其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
命令`git rm` 用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失<font color="#6638F0">**最近一次提交后你修改的内容**。</font>

# 远程仓库
## 添加公钥
`ssh-keygen -t rsa -C "youremail@example.com"`

## 添加远程库
1. git 初始化 `git init` 
2. 添加远程库 `git remote add origin https://github.com/life4living/CoralMicrobiome.git`
3. `git push -u origin master` 本地库推送到远程库, 把当前分支`master` 推送到远程.由于远程库是空的，我们第一次推送`master` 分支时，加上了 `-u` 参数，Git不但会把本地的`master` 分支内容推送的远程新的`master` 分支，还会把本地的`master`分支和远程的`master` 分支关联起来，在以后的推送或者拉取时就可以简化命令。
4. 今后该目录本地提交 通过`git push origin master` 


## 从远程库克隆 `git clone`
`git clone git@github.com:life4living/r4ds.git`

github 可以使用 `https` 形式 `https://github.com/life4living/CoralMicrobiome.git`, 同样可以使用`ssh` 形式 `git@github.com:life4living/CoralMicrobiome.git` . 使用 `https` 除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用`ssh` 协议而只能用 `https`. 


# 分支管理
## introducion
![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908633976bb65b57548e64bf9be7253aebebd49af000/0)

## 相关命令
查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

`fast forward` 模式合并某分支到当前分支：`git merge <name>`
普通模式合并分支,并添加message`git merge --no-ff -m "merge with no-ff" dev`

删除分支：`git branch -d <name>`

强行删除分支 `git branch -D <name>`

查看分支合并图: `git log --graph --pretty=oneline --abbrev-commit`
  

### 分支管理的应用场景
需要协同工作的项目, 可以创建自己的分支, 在自己的分支上进行Coding, 完成后合并到主分支上.

## 创建于合并分支
### 创建分支
当我们创建新的分支，例如`dev` 时，Git新建了一个指针叫`dev` ，指向 `master` 相同的提交，再把`HEAD` 指向`dev` ，就表示当前分支在`dev` 上：
![创建dev分支-c](https://d.pr/i/JTEAAx+)
从现在开始，对工作区的修改和提交就是针对`dev` 分支了，比如新提交一次后，`dev` 指针往前移动一步，而`master` 指针不变：
![dev提交-c](https://d.pr/i/SceDzM+)

### 分支合并
`dev` 上的工作完成后，就可以把`dev`合并到`master` 上:
1. 直接把`master` 指向`dev` 的当前提交，就完成了合并,合并后可以选择将`dev` 分支删除,仅保留`master` 分支.
![分之合并-c](https://d.pr/i/CKVUvZ+)

```bash
# 创建分支
git checkout -b dev

# -b 表示创建分支并切换
git branch dev
git checkout dev

# 查看分支
git branch
* dev
  master
# * 表示当前分支

# 合并分支
git merge dev #当前分支为master
# Fast-forward 表示这次合并是“快进模式”，也就是直接把master指向dev的当前提交

# 删除分支
git branch -d dev

```

## 解决冲突
<font color="#F78AE0">**当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。**</font>


```bash
git checkout master
# Your branch is ahead of 'origin/master' by 1 commit.
# 当前master分支比远程的master分支要超前1个提交
```


### 冲突
`master` 和 `feature1` 分支分别有新的提交, Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，
![master文件修改->冲突-c](https://d.pr/i/qNfaDA+)
![冲突内容-c](https://d.pr/i/2FPLQU+)
### 解决冲突
`master` 下修改保存,提交
![-c](https://d.pr/i/ulT7PZ+)

## 分支管理策略
通常，合并分支时，如果可能，Git会用`Fast forward` 模式，但这种模式下，删除分支后，会丢掉分支信息。如果要强制禁用`Fast forward` 模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
<div style="background-color: #D8EAF2; color: #497091; margin-bottom: 1px; padding: 1% 1% 1% 1%; border-radius: 5px;">合并分支时，加上`--no-ff` 参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward` 合并就看不出来曾经做过合并。
</div>
![分支合并的两种模式-c](https://d.pr/i/pbtw1Y+)
<center>
![协同开发下,分支模式](https://d.pr/i/6WG8Kp+)
<br>协同开发下的分支
</center>

## bug分支 建立stash隐藏工作区
修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
当手头工作没有完成时，先把工作现场(需要先`add`)`git stash` 一下，然后去修复bug，修复后，再`git stash pop` ，回到工作现场。

```bash
git stash list # 查看隐藏区
git stash apply #将隐藏区 加载到工作区恢复之前add状态
git stash drop #删除隐藏区
git stash pop = git stash apply&drop
#可以多次git stash, 
#恢复时使用
git stash apply stash@{0}
```

## 多人协作

* 查看远程库信息，使用`git remote -v` ；
* 本地新建的分支如果不推送到远程，对其他人就是不可见的；
* 从本地推送分支，使用`git push origin branch-name` ，如果推送失败，先用`git pull` 抓取远程的新提交；
* 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name` ，本地和远程分支的名称最好一致；
* 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name` ；
* 从远程抓取分支，使用`git pull` ，如果有冲突，要先处理冲突。

## Rebase (变基) **❓**
多人在同一个分支上协作时，很容易出现冲突。即使没有冲突，后push的不得不先pull，在本地合并，然后才能push成功。每次合并再push后，分支会变得非常乱, 历史记录不是一条干净的直线. `rebase` 解决该问题

* `rebase` 操作可以把本地未push的分叉提交历史整理成直线；
* `rebase` 的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

  

# 标签管理

## 标签管理的目的

发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（类似分支, 但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。<font color="#F78AE0">**Tag是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。**</font>

## 创建标签
  * 命令`git tag <tagname>` 用于新建一个标签，默认为`HEAD` ，也可以指定一个commit id；
  * 命令`git tag -a <tagname> -m "blablabla..."` 可以指定标签信息；
  * 命令`git tag` 可以查看所有标签。
  * 命令`git show <tag>` 可以查看制定tag/commit
  
## 操作标签
  * 命令` git push origin <tagname>` 可以推送一个本地标签；
  * 命令` git push origin --tags` 可以推送全部未推送过的本地标签；
  * 命令` git tag -d <tagname>` 可以删除一个本地标签；
  * 命令 `git push origin :refs/tags/<tagname>` 可以删除一个远程标签。

# 使用Github

* 在GitHub上，可以任意Fork开源仓库；
* 自己拥有Fork后的仓库的读写权限；
* 可以推送pull request给官方仓库来贡献代码。

# 自定义Git

Git 显示颜色 `git config --global color.ui true`

## 忽略特殊文件
[github官方gitignore配置](https://github.com/github/gitignore)

> 忽略文件的原则是：
	1. 忽略操作系统自动生成的文件，比如缩略图等；
	2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的`.class` 文件；
	3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。
	
强制添加文件 `git add -f App.class` 

```bash
git check-ignore -v .DS_Store
.gitignore:2:.DS_Store	.DS_Store
# Git会告诉我们，.gitignore的第2行规则忽略了该文件
```

<font color="#F78AE0">**文件本身要放到版本库里，并且可以对 `.gitignore` 做版本管理. **</font>

## 配置别名

```bash
Git Config --Global Alias.Co Checkout
Git Config --Global Alias.Ci Commit
Git Config --Global Alias.Br Branch
# `--global`参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
`git lg`
![](https://d.pr/i/3GAYgS+)

配置Git的时候，加上`--global` 是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。每个仓库的Git配置文件都放在`.git/config` 文件中.





