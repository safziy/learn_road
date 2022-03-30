# Git命令

官网文档 [http://git-scm.com/docs/](http://git-scm.com/docs/)

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

##### <a id='diff'>git diff</a>
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
git diff [first-branch]...[second-branch]
```

##### <a id='commit'>git commit</a>
`git commit` 命令将暂存区内容添加到本地仓库中
```shell
# 提交暂存区到本地仓库中
git commit -m [message]
# 提交暂存区的指定文件到仓库区
git commit [file1] [file2] ... -m [message]
# -a 参数设置修改文件后不需要执行 git add 命令，直接来提交
git commit -a
```

##### <a id='reset'>git reset</a>
`git reset` 命令用于回退版本，可以指定退回某一次提交的版本。</br>
`git reset` 命令语法格式如下：</br>
  > `git reset [--soft | --mixed | --hard] [HEAD]`
- --mixed 为默认，可以不用带该参数，用于重置暂存区的文件与上一次的提交(commit)保持一致，工作区文件内容保持不变。</br>
  > `git reset [HEAD]`</br>

  示例
  ```shell
  git reset HEAD^            # 回退所有内容到上一个版本  
  git reset HEAD^ hello.php  # 回退 hello.php 文件的版本到上一个版本  
  git reset 052e             # 回退到指定版本
  ```
- --soft 参数用于回退到某个版本</br>
  > `git reset --soft HEAD`
  
  示例
  ```shell
  git reset --soft HEAD~3 # 回退上上上一个版本
  ```
- --hard 参数撤销工作区中所有未提交的修改内容，将暂存区与工作区都回到上一次版本，并删除之前的所有信息提交
  > `git reset --hard HEAD`
  
  示例
  ```shell
  git reset -–hard HEAD~3           # 回退上上上一个版本  
  git reset –-hard bae128           # 回退到某个版本回退点之前的所有信息。
  git reset --hard origin/master    # 将本地的状态回退到和远程的一样
  ```
  > 注意：谨慎使用 –hard 参数，它会删除回退点之前的所有信息。

  HEAD 说明
  - HEAD 表示当前版本
  - HEAD^ 上一个版本
  - HEAD^^ 上上一个版本
  - HEAD^^^ 上上上一个版本
  - 以此类推...
    
  可以使用 ～数字表示
  - HEAD~0 表示当前版本
  - HEAD~1 上一个版本
  - HEAD^2 上上一个版本
  - HEAD^3 上上上一个版本
  - 以此类推...


##### <a id='rm'>git rm</a>
`git rm` 命令用于删除文件。</br>
如果只是简单地从工作目录中手工删除文件，运行 `git status` 时就会在 `Changes not staged for commit` 的提示。</br>
git rm 删除文件有以下几种形式：
1. 将文件从暂存区和工作区中删除：
> `git rm <file>`
2. 如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 `-f`
> `git rm -f <file>`
3. 如果想把文件从暂存区域移除，但仍然希望保留在当前工作目录中，换句话说，仅是从跟踪清单中删除，使用 `--cached` 选项即可
`git diff` 有两个主要的应用场景。
> `git rm --cached <file>`
4. `-r`可以递归删除，即如果后面跟的是一个目录做为参数，则会递归删除整个目录中的所有子目录和文件
> `git rm –r *`

##### <a id='mv'>git mv</a>
`git mv` 命令用于移动或重命名一个文件、目录或软连接。
> `git mv [file] [newfile]`

如果新文件名已经存在，但还是要重命名它，可以使用 `-f` 参数：
> `git mv -f [file] [newfile]`

##### <a id='log'>git log</a>
查看历史提交记录。</br>
在使用 Git 提交了若干更新之后，又或者克隆了某个项目，想回顾下提交历史，我们可以使用 `git log` 命令查看。</br>
> `git logq`

我们可以用 `--oneline` 选项来查看历史记录的简洁的版本。</br>
> `git log --oneline`

我们还可以用 `--graph` 选项，查看历史中什么时候出现了分支、合并。以下为相同的命令，开启了拓扑图选项：
> `git log --graph`

也可以用 `--reverse` 参数来逆向显示所有日志
> `git log --reverse`

如果只想查找指定用户的提交日志可以使用命令：`git log --author`, 例如，比方说我们要找 Git 源码中 Linus 提交的部分：
> `git log --author=safziy`

更多git log命令可以查看 [http://git-scm.com/docs/git-log](http://git-scm.com/docs/git-log)

##### <a id='blame'>git blame</a>
查看指定文件的修改记录</br>
> `git blame <file>`

##### <a id='remote'>git remote</a>
`git remote` 命用于在远程仓库的操作。</br>
```shell
# 显示所有远程仓库：
git remote -v
# 显示某个远程仓库的信息：
git remote show [name]
# 添加远程版本库
git remote add [shortname] [url]
# 删除远程仓库
git remote rm [name]
# 修改仓库名
git remote rename [old_name] [new_name]
```


##### <a id='fetch'>git fetch</a>
`git fetch` 命令用于从远程获取代码库。</br>
```shell
# 提取更新的数据
git fetch [alias]
# 尝试合并到当前分支
git merge [alias]/[branch]
```


##### <a id='pull'>git pull</a>
`git pull` 命令用于从远程获取代码并合并本地的版本。</br>
`git pull` 其实就是 `git fetch` 和 `git merge FETCH_HEAD` 的简写。 命令格式如下：</br>
> `git pull <远程主机名> <远程分支名>:<本地分支名>`
```shell
# 更新操作：
git pull
git pull origin
# 将远程主机 origin 的 master 分支拉取过来，与本地的 develop 分支合并。
git pull origin master:develop
# 如果远程分支是与当前分支合并，则冒号后面的部分可以省略。
git pull origin master
```


##### <a id='push'>git push</a>
`git push` 命用于从将本地的分支版本上传到远程并合并。</br>
命令格式如下：</br>
> `git push <远程主机名> <本地分支名>:<远程分支名>`

如果本地分支名与远程分支名相同，则可以省略冒号：
> `git push <远程主机名> <本地分支名>`

示例</br>
```shell
# 以下命令将本地的 master 分支推送到 origin 主机的 master 分支。
git push origin master
# 相等于：
git push origin master:master
# 如果本地版本与远程版本有差异，但又要强制推送可以使用 `--force` 参数：
git push --force origin master
# 删除主机的分支可以使用 `--delete` 参数，以下命令表示删除 origin 主机的 master 分支：
git push origin --delete master
```