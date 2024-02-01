---
title: 初识 Git
tags: ["Git"]
image: /static/images/git.png
pubDate: 2024-1-31
featured: false
---

# 初识 Git

用了一天就把 b 站上的 git 视频看完了，看完之后收获颇丰，怎么没早知道这么好的东西，还是怪自己求知欲太低，先把常用的一些命令还有基础知识总结一下吧。

### 一.Git的三种状态

1. **已修改（modified）**:表示修改了文件，但还没有保存到数据库中。
2. **已暂存（staged）**：表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。
3. **已提交（commited）**：表示数据已经安全地保存在本地数据库中。

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240131/截屏2024-01-31-22.47.01.12w6aq7xbrmo.png"/>

**总结为：工作区——>暂存区——>本地仓库**

### 二.安装 Git

 在 macos 上安装（安装 xcode 即可），检测是否安装成功：

```powershell
$ git -v
```

### 三.初次运行 Git 前的配置

1. 用户信息

安装 Git 之后，要做的第一件事就是设置你的用户名和邮件地址。这一点很重要，因为每一个 Git 提交都会用这些信息，它们会写入到你的每次提交中，不可更改：

```powershell
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

这些配置是全局的，也就是说，它们将应用于你的系统上的所有 Git 仓库，除非你在特定的仓库内部覆盖它们（使用无 --global 选项的 git config 命令）。

例如：

```powershell
git config user.name "John Doe"
git config user.email johndoe@example.com
```

这些配置项通常在你首次安装 Git 并准备开始作出贡献时设置如果你尝试在没有设置用户名和用户邮箱的情况下进行提交，Git 会报错，因为这些信息是构成一次有效 Git 提交的必要部分。此外，确保所提供的邮箱与你在任何托管服务（如 GitHub、GitLab 或 Bitbucket）上使用的邮箱一致，以便将你的提交和你的账户相关联。

### 四.获取 Git 仓库

1. 在已存在目录中初始化仓库

```powershell
$ cd /Users/user/my_project
$ git init
```

初始化后，通过命令：

```powershell
ls -a
```

可以检查目录下是否出现.git文件

2. 克隆现有的仓库

```powershell
$ git clone https://github.com/libgit2/libgit2
```

这会在当前目录下创建一个名为“libgit2”的仓库

也可以通过以下命令自定义本地仓库的名字：

```powershell
$ git clone https://github.com/libgit2/libgit2 mylibgit
```

Git 支持多种数据传输协议。 上面的例子使用的是 https:// 协议，不过你也可以使用 git:// 协议或者使用 SSH 传输协议，比如 user@server:path/to/repo.git 。

### 五.记录每次更新到仓库

工作目录下的每一个文件都不外乎这两种状态：**已跟踪** 或 **未跟踪。**已跟踪的文件是指那些被纳入版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能是未修改，已修改或已放入暂存区。简而言之，**已跟踪的文件就是 Git 已经知道的文件**。

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240131/截屏2024-01-31-23.14.31.2q6kulgqr960.png"/>

1. 检查当前文件状态

可以用 git status 命令查看哪些文件处于什么状态。

```powershell
$ git status
```

2. 跟踪新文件和暂存已修改的文件

使用命令 ``git add`` 开始跟踪一个文件。

```powershell
$ git add README.md
```

也可以使用以下命令将当前目录的所有更改增加到暂存区中：

```powershell
$ git add .
```

3. 忽略文件

一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动生成的文 件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以创建一个名为 ``.gitignore`` 的文件，列出要忽略的文件的模式。 来看一个实际的 ``.gitignore`` 例子:

```powershell
vim .gitignore
echo hello.log > .gitignore
```

将文件名添加到`.gitignore`文件中即可。

4. 查看已暂存和未暂存的修改

查看工作区和暂存区之间的差异：

```powershell
$ git diff
```

查看暂存区和最后一次提交的文件差异：

```powershell
$ git diff --staged
```

5. 提交更新

现在的暂存区已经准备就绪，可以提交了。 在此之前，请务必确认还有什么已修改或新建的文件还没有 ``git add`` 过， 否则提交的时候不会记录这些尚未暂存的变化。 这些已修改但未暂存的文件只会保留在本地磁盘。 所以，每次准备提交前，先用 ``git status`` 看下，你所需要的文件是不是都已暂存起来了。

```powershell
$ git commit
```

添加描述：

```powershell
$ git commit -m "change sth"
```

6. 跳过使用暂存区域

给 ``git commit`` 加上 -a 选项，Git 就会自动把所有已经跟踪过的文件暂存 起来一并提交，从而跳过``git add``步骤:

```powershell
$ git commit -a -m "change sth"
```

或者

```powershell
$ git commit -am "change sth"
```

7. 移除文件

要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除(确切地说，是从暂存区域移除)，然后提交。

若是只从本地删除，则还需要进行 ``git add``，使用如下命令可直接同时删除本地和暂存区文件且不需要 ``git add``：

```powershell
$ git rm hello.txt
```

8. 重命名文件

```powershell
$ git mv README.md README
```

相当于执行了下面三条命令：

```powershell
$ mv README.md README
$ git rm README.md
$ git add README
```

### 六.查看提交历史

```powershell
$ git log
```

不传入任何参数的默认情况下，``git log``会按时间先后顺序列出所有的提交，最近的更新排在最上面。

显示最近两次提交的差异：

```powershell
$ git log -p -2
```

将每个提交放在一行显示：

```powershell
$ git log --ongline
```

### 七.撤销操作

有时候我们提交完了才发现漏了几个文件没有添加，或者提交信息写错了。此时，可以运行如下命令重新提交：

```powershell
$ git commit --amend
```

与其说是修复旧提交，倒不如说是完全用一个新的提交替换旧的提交。

1. 取消暂存的文件

```powershell
$ git reset HEAD hello.txt
```

2. 撤销对文件的修改

```powershell
$ git checkout -- hello.txt
```

你对这个文件在本地的任何修改都会消失，Git 会用在最近提交的版本覆盖掉他（暂存区的版本）。

### 八.远程仓库的使用

1. 查看远程仓库

列出远程服务器的简写：

```powershell
$ git remote 
```

显示远程仓库使用 Git 保存的简写及其对应的 URL：

```powershell
$ git remote -v
```

2. 添加远程仓库

运行``git remote add shortname url``添加一个新的远程仓库，同时指定一个方便使用的简写：

```powershell
$ git remote add git_learn git@github.com:SUNSIR007/git_learn.git
```

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-07.31.34.295n2e9nvxj4.png"/>

现在你可以在命令行中使用字符串git_learn来代替整个 URL。

3. 从远程仓库中抓取与拉取

```powershell
$ git fetch <remote>
```

这个命令会访问远程仓库，从中拉取所有你还没有的数据。执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并并查看。

如果你使用 clone 命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以“origin”为简写。所以，``git fetch origin``会抓取克隆（或上一次抓取）后新推送的所有工作。必须注意 git fetch命令只会讲数据下载到你的本地仓库，它并不会自动合并或者修改你当前的工作。当准备好时你必须手动将其合并入你的工作。

运行``git pull``通常会从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支。

4. 推送到远程仓库

当你想将 main 分支推送到 origin 服务器时，可以运行如下命令：

```powershell
$ git push origin main
```

5. 查看某个远程仓库

```powershell
$ git remote show origin
```

6. 远程仓库的重命名与移除

将 pb 重命名为 paul

```powershell
$ git remote rename pb paul
```

如果想要移除一个远程仓库，可以使用如下命令：

```powershell
$ git remote remove paul
```

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-07.52.08.2srq51hfaay0.png"/>

### 九.Git 别名

```powershell
$ git config --global alias.co checkout
$ git config --global alias.ci commit
```

co → checkout, ci → commit

或者给一串命令起别名：

```powershell
$ git config --global alias.unstage 'reset HEAD --'
```