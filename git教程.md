## Git教程

**Tutorial for Git**

Auth: **月と猫 - LunaNeko** (GitHub: LunaticLegacy) 

Environment: **Any** 

本文作为个人笔记，可作为新手快速入门教程食用，但**可能存在错误**。所有能被写进这个.md文件里的都是我亲自实验过或者来自文档内容的。

心理准备：Git是一个**按批次更改文件的、基于提交历史记录和分支**的版本控制系统。

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
- 2.1 
-

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
```bash
PS E:\Linux-Commands-Table-for-Absolute-Noob> git commit -m "2025/7/7: 增加文件：git教程.md。"
[main cd01ad3] 2025/7/7: 增加文件：git教程.md。
 1 file changed, 72 insertions(+)
 create mode 100644 "git\346\225\231\347\250\213.md"
```

当文件被更改后，使用`git status`可看被更改的文件。

使用：
```bash
git push <repo_name> <branch_name>
```
以将代码推送到远程仓库。其中\<repo_name\>项需要**提前设置**。具体见[后续]()。

### 1.4 分支是什么？

分支（Branch），用于从当前环境分离一份代码，并独立地进行开发。在新的Git仓库被创建后，一般会生成一个主分支`master`或`main`*（取决于个人配置）*。

使用：
```bash
git branch
```
以查看现在有多少分支。

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


