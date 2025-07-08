## Git教程

**Tutorial for Git**

Auth: **月と猫 - LunaNeko** (GitHub: LunaticLegacy) 

Written in environment: Windows 11, git version: 2.47.0 

本文作为个人笔记，可作为新手快速入门教程食用，但**可能存在错误**。所有能被写进这个.md文件里的都是我亲自实验过或者来自文档内容的。

心理准备：Git是一个**按批次更改文件的、基于提交节点**的版本控制系统。

**本文适合有一定shell使用基础的人食用，无论是Windows Powershell还是Linux的bash。**

## 你需要做的第一件事：

0. 下载并安装Git。*（这个不用说都知道了啊）*
1. 让命令行前往某处，执行如下命令：
```bash
git clone https://github.com/LunaticLegacy/Linux-Commands-Table-for-Absolute-Noob.git
```
很好，做完这条后，你现在将整个教程的git克隆下来了，以**文件夹形式**。且文件夹名字和git的仓库名相同。


## 教程目录：

1. 本地仓库
- 1.1 创建仓库
- 1.2 管理文件
- 1.3 提交和推送
- 1.4 分支是什么？

2. 远程仓库
- 2.1 建立并配置远程仓库
- 2.2 将代码推送到远程仓库
- 2.3 从远程仓库同步代码

## 1. 本地仓库

### 1.1 创建仓库

可以使用：
```bash
git init
```
创建一个新的空白仓库，这个仓库的默认名字是：**.git**。

也可以使用：
```bash
git clone <repo_url>
```
以从指定仓库拉取文件，并在本地创建一个仓库。上述内容就是一个例子。

关于拉取仓库时需要的远程配置**在后续。**（WIP）


### 1.2 管理文件

使用：
```bash
git add <file_name>
```
以将文件添加到当前仓库的追踪。

使用：
```bash
git status
```
以查看当前仓库的修改状态。

题主在将本教程使用`git add git教程.md`后，使用`git status`查看更改，会显示类似如下内容：
```
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   "git\346\225\231\347\250\213.md"
```
可以看出，非ASCII字符会被显示为UTF-8编码的原始代码。

使用：
```bash
git rm <file_name>
```
以在记录中删除文件。注意：在git中被删除的文件**不会真正地被从硬盘中删除**，只是从此开始不被继续追踪。

### 1.3 提交和推送

使用：
```bash
git commit
```
以提交当前的修改。

*还记得如何查看哪些文件被修改吗？看[这里](#12-管理文件)*

可以使用：
```bash
git commit -m "commit message"
```
以描述当前改动。一般在大型项目合作中，对改动的描述是义务性质的。

例如：
```powershell
PS E:\Linux-Commands-Table-for-Absolute-Noob> git commit -m "2025/7/7: 增加文件：git教程.md。"
[main cd01ad3] 2025/7/7: 增加文件：git教程.md。
 1 file changed, 72 insertions(+)
 create mode 100644 "git\346\225\231\347\250\213.md"
```

当文件被更改后，使用`git status`可看被更改的文件。

如果需（在当前HEAD）进行增量更新而不创建新的git提交，使用：
```bash
git commit --amend [-m <"new message">] [-F <file_name>]
```
即可。其中-F和-m选项不得同时出现。

```bash
git commit --amend -F "./git教程.md"
```
以增量更新.

使用：
```bash
git push <repo_name> <branch_name>
```
以将代码推送到远程仓库。其中\<repo_name\>项需要**提前设置**。具体见[后续]()。

### 1.4 分支是什么？
Git基于提交时间线节点进行代码管理。创建分支是为了从当前环境分离一份备份，并根据本备份独立开发。

在新的Git仓库被创建后，一般会生成一个主分支`master`或`main`*（取决于个人配置）*。

使用：
```bash
git log
```
以查看当前分支的提交情况。但一般会使用：`git log --oneline`以进行更多分支。

例如：
```powershell
PS E:\Linux-Commands-Table-for-Absolute-Noob> git log --oneline
8880835 (HEAD -> main) Create and Update git教程.md
c389d94 Update README.md (#7)
3666721 Update SSH教程.md
d0b85b8 Update SSH教程.md
bd01652 Update SSH教程.md (#6)
c7e87c4 Update README.md (#5)
524cf61 Update tmux教程.md (#4)
304cb9f Upload tmux教程.md
...（后略）
```
其中可以看到这些部分，在上例子中，节点ID`8880835`为当前HEAD所在位置——即正在被修改的节点。

使用：
```bash
git branch
```
以查看现在有多少分支，以及自己当前所处的分支。

使用：
```bash
git branch <new_branch_name>
```
以新建分支。

使用：
```bash
git branch -d <branch_name>
```
以删除指定分支。注意：只有在当前分支下没有修改的分支才能删除。即**只能从叶分支开始删除分支**。

**分支被删除是不可逆的。*（别问，问就是血的教训）***

使用：
```bash
git checkout <branch_name>
```
以签出，即**将当前工作状态切换到指定分支**。

签出分支时必须保证当前没有文件正在更改。

使用：
```bash
git merge <branch_name>
```
以将指定的（其他）分支合并到当前分支。

## 2. 远程仓库

远程仓库是用于云端保存代码的在线仓库。本示例中将使用GitHub作为远程仓库。

### 2.1 配置远程仓库


