# Git命令

### 1.查看和修改配置信息
Git 提供了一个叫做 `git config` 的工具，专门用来配置或读取相应的工作环境变量

这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：
- /etc/gitconfig 文件：系统中对所有用户都普遍适用的配置。若使用 git config 时用 --system 选项，读写的就是这个文件。
- ~/.gitconfig 文件：用户目录下的配置文件只适用于该用户。若使用 git config 时用 --global 选项，读写的就是这个文件。
- 当前项目的 Git 目录中的配置文件（也就是工作目录中的 .git/config 文件）：这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 .git/config 里的配置会覆盖 /etc/gitconfig 中的同名变量。

在 Windows 系统上，Git 会找寻用户主目录下的 .gitconfig 文件。主目录即 $HOME 变量指定的目录，一般都是 C:\Documents and Settings\$USER。

此外，Git 还会尝试找寻 /etc/gitconfig 文件，只不过看当初 Git 装在什么目录，就以此作为根目录来定位。
```shell
# 查看所有的配置
git config --list
# 查看指定的配置
git config user.name
# 配置个人的用户名称和电子邮件地址
git config -global user.name "test"
git config -global user.email test.163.com
# 设置Git默认使用的文本编辑器，一般可能会是 Vi 或者 Vim。如果你有其他偏好，比如 Emacs 的话，可以重新设置
git config -global core.editor emacs
# 差异分析工具,Git 可以理解 kdiff3，tkdiff，meld，xxdiff，emerge，vimdiff，gvimdiff，ecmerge，和 opendiff 等合并工具的输出信息
git config --global merge.tool vimdiff
```

### 2.Git基本操作

Git 的工作就是创建和保存你项目的快照及与之后的快照进行对比。</br>
Git 常用的是以下 6 个命令：git clone、git push、git add 、git commit、git checkout、git pull

![](../images/git-command.jpeg "git工作流")
> 说明：</br>
> workspace：工作区</br>
> staging area：暂存区/缓存区</br>
> local repository：版本库或本地仓库</br>
> remote repository：远程仓库

#### 创建仓库命令

下表列出了 git 创建仓库的命令：

命令|说明
---|---
[`git init`](#init)|初始化仓库
[`git clone`](#clone)|拷贝一份远程仓库，也就是下载一个项目

#### 提交与修改

Git 的工作就是创建和保存你的项目的快照及与之后的快照进行对比。

下表列出了有关创建与提交你的项目的快照的命令：

命令|说明
---|---
[`git add`](#add)|添加文件到暂存区
[`git status`](#status)|查看仓库当前的状态，显示有变更的文件
[`git diff`](#diff)|比较文件的不同，即暂存区和工作区的差异
[`git commit`](#commit)|提交暂存区到本地仓库
[`git reset`](#reset)|回退版本
[`git rm`](#rm)|删除工作区文件
[`git mv`](#mv)|移动或重命名工作区文件

#### 提交日志

命令|说明
---|---
[`git log`](#log)|查看历史提交记录
[`git blame <file>`](#blame)|以列表形式查看指定文件的历史修改记录

#### 远程操作

命令|说明
---|---
[`git remote`](#remote)|远程仓库操作
[`git fetch`](#fetch)|从远程获取代码库
[`git pull`](#pull)|下载远程代码并合并
[`git push`](#push)|上传远程代码并合并

#### 命令说明

##### <a id='init'>git init</a>
Git 使用 `git init` 命令来初始化一个 Git 仓库，Git 的很多命令都需要在 Git 的仓库中运行，所以 git init 是使用 Git 的第一个命令。</br>
在执行完成 `git init` 命令后，Git 仓库会生成一个 .git 目录，该目录包含了资源的所有元数据，其他的项目目录保持不变。
```shell
git init
git init <newRepo>
```

##### <a id='clone'>git clone</a>
`git clone` 拷贝一个 Git 仓库到本地，让自己能够查看该项目，或者进行修改

拷贝项目命令格式如下：
```shell
git clone <repo>
# 如果我们需要克隆到指定的目录，可以使用以下命令格式
git clone <repo> <directory>
```

##### <a id='add'>git add</a>
`git add` 命令可将该文件添加到暂存区
```shell
# 添加一个或多个文件到暂存区
git add [file1] [file2] ...
# 添加指定目录到暂存区，包括子目录
git add [dir]
# 添加当前目录下的所有文件到暂存区
git add .
```

##### <a id='status'>git status</a>
`git status` 命令用于查看在你上次提交之后是否有对文件进行再次修改
```shell
git status
# 通常我们使用 -s 参数来获得简短的输出结果
git status -s
```

##### <a id='status'>git diff</a>
`git diff` 命令比较文件的不同，即比较文件在暂存区和工作区的差异。</br>
`git diff` 命令显示已写入暂存区和已经被修改但尚未写入暂存区文件的区别。</br>
`git diff` 有两个主要的应用场景。
- 尚未缓存的改动：git diff
- 查看已缓存的改动： git diff --cached
- 查看已缓存的与未缓存的所有改动：git diff HEAD
- 显示摘要而非整个 diff：git diff --stat
```shell
# 显示暂存区和工作区的差异:
git diff [file]
# 显示暂存区和上一次提交(commit)的差异:
git diff --cached [file]
git diff --staged [file]
# 显示两次提交之间的差异:
$ git diff [first-branch]...[second-branch]
```