# 引言
## 本地版本控制系统
在本地计算机采用数据库记录文件的历次改动
缺点是无法应用于多人协作

## 集中式版本控制系统
在服务器上采用数据库记录文件的历次改动，各个客户端连接到服务器，取出最新的文件，修改后，提交更新
缺点是时刻依赖于服务器，当服务器宕机时，所有人都无法工作，且数据库可能永久丢失

## 分布式版本控制系统
同集中式版本控制系统一样，在服务器上采用数据库记录文件的历次改动，区别是客户端从服务器提取最新版本时，不只是取出最新文件，而是把整个数据库都取出来。即使服务器发生故障，依然可以通过客户端本地数据库进行恢复。也就是说，从服务器拉取仓库到本地客户端之后，在离线状态下除了不能push，不影响其它类型的工作。

## 存储
其它的版本控制系统保存的是每个文件，以及各个版本的文件改动。git存储的是每个版本的文件快照

## 分区
从服务器拉取仓库之后，最新版本的文件保存在本地磁盘上供编辑，此时这些文件处于工作区；
修改完文件后，通过git add操作，编辑的文件从工作区进入暂存区；
git commit会将暂存区的所有文件保存到仓库，没有进入暂存区的文件不会被保存到git数据库。
在工作区修改文件后，文件的状态是“修改”；
对“修改”的文件执行git add之后，文件的状态是“暂存”；
对“暂存”的文件执行git commit之后，文件的状态是“提交”（注意这里的提交只是把文件提交到本地仓库，并没有提交到服务器仓库）

## 操作
git有命令行模式，也有相应的GUI软件。

# 安装
可以在windows系统上安装github desktop；
安装完git后需要进行配置；

## 配置
安装完git之后，要修改配置文件，配置文件有三个，一个位于git安装路径下，记为level1；一个位于个人home目录下，记为level2，一个位于每个仓库中，记为level3.
git config用于编辑配置文件，跟--system选项编辑的是level1，跟--global选项编辑的是level2，跟--local选项编辑的是level3.
示例：
git config --global user.name "albert"
git config --global user.email albert@163.com

git config --list用于列出当前配置


# 操作
## 创建
git init初始化
git add添加文件
git commit提交

## 克隆
git clone https://github.com/libgit2/libgit2 mylibgit

## 查看两个版本之间修改了哪些文件（只列出文件名，不展示细节）
git diff --name-status xxxxxxxxx yyyyyyyyy

## 回退到某个版本
git reset --hard xxxxxxxxxx

## git add
这是个多功能命令：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。
运行了 git add 之后又作了修订的文件，需要重新运行 git add 把最新版本重新暂存起来。
git add 命令使用文件或目录的路径作为参数；如果参数是目录的路径，该命令将递归地跟踪该目录下的所有文件

## .gitignore
一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以创建一个名为 .gitignore的文件，列出要忽略的文件的模式。
一个 .gitignore 文件的例子：

\# 忽略所有的 .a 文件

*.a


\# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件

!lib.a


\# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO

/TODO


\# 忽略任何目录下名为 build 的文件夹

build/


\# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt

doc/*.txt


\# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件

doc/**/*.pdf

## git diff
此命令比较的是工作目录中当前文件和暂存区域快照之间的差异。 也就是修改之后还没有暂存起来的变化内容。
若要查看已暂存的将要添加到下次提交里的内容，可以用 git diff --staged 命令。 这条命令将比对已暂存文件与最后一次提交的文件差异.

## git commit
git commit -a -m 'added new benchmarks'

git commit --amend用于覆盖上次提交

## git rm
从已跟踪文件清单中移除（确切地说，是从暂存区域移除），并连带从工作目录中删除指定的文件.
如果要删除之前修改过或已经放到暂存区的文件，则必须使用强制删除选项 -f（译注：即 force 的首字母）。 这是一种安全特性，用于防止误删尚未添加到快照的数据.
另外一种情况是，我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。 换句话说，你想让文件保留在磁盘，但是并不想让 Git 继续跟踪。 当你忘记添加 .gitignore 文件，不小心把一个很大的日志文件或一堆 .a 这样的编译生成文件添加到暂存区时，这一做法尤其有用。 为达到这一目的，使用 --cached 选项：git rm --cached README

## git push
git push origin master将当前clone下来的仓库master分支push到远程服务器仓库（名字简称是origin）的master分支。如果遇到没有权限push的话，使用git config receive.denyCurrentBranch ignore对服务器仓库进行配置

## git remote
查看远程服务器仓库名字，如果当前仓库是通过git clone得到，则远程服务器仓库会自动被命名为origin。可以使用git remote rename origin xxx将远程服务器仓库名字从origin修改成xxx

## git tag
先把当前所有的改动都提交，然后使用git tag -a xxx -m "yyy"，将这次提交打了个标签xxx，这个标签的描述是yyy


