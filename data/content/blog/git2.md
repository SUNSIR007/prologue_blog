---
title: Git分支
tags: ["Git"]
image: /static/images/git.png
pubDate: 2024-2-1
featured: false
---

# Git分支

几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离开来， 以免影响开发主线。 

### 一.分支简介

Git 是如何保存数据的：**GIt 保存的不是文件的变化或者差异，而是一系列不同时刻的快照。**

在进行提交操作时，Git 会保存一个提交对象（commit object），包含一个指向内容快照的指针，还包含了作者的姓名和邮箱，提交时输入的信息以及指向它的父对象的指针。（首次提交产生的提交对象没有父对象）

1. 暂存操作（git add .）：为每一个文件计算校验和（哈希算法），然后会把当前版本的文件快照保存到 Git 仓库中（git 中使用 blob 对象来保存他们），最终将校验和加入到暂存区域等待提交。
2. 提交操作（git commit -m “”）：GIt 会先计算每一个子目录的校验和，然后在 Git 仓库中这些校验和保存为树对象。随后，Git 便会创建一个提交对象，它除了包含上面提到的那些信息外，还包含指向这个树对象的指针。如此一来，Git 就可以在需要的时候重现此次保存的快照。

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-10.14.35.6vlv4kpy7y00.png"/>

做些修改后再次提交，那么这次产生的提交对象会包含一个指向上次提交对象（父对象）的指针。

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-10.24.17.4ybdlbej9rw0.png"/>

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-10.26.30.2kpnfwbngmi0.png"/>

### 二.分支创建

Git 是如何创建新分支的：答案是它只是为你创建了一个可以移动的指针。

```powershell
$ git branch testing
```

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-10.28.53.4bco97vmdqy0.png"/>

Git是如何知道当前在哪一个分支上的：答案是有一个名为`HEAD`的特殊指针，它指向当前所在的本地分支（可以理解为当前分支的别名）。

执行完`git branch testing`命令后，你仍然在 master 分支上，因为`git branch`命令仅仅创建了一个新分支，并不会自动切换到新分支去。

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-10.34.34.exts1o76o4g.png"/>

使用`git log —oneline —decorate`可以查看当前各个分支指向的对象。

### 三.分支切换

切换到一个已经存在的分支：

```powershell
$ git checkout testing
```

或者

```powershell
$ git switch testing
```

这样 HEAD就指向 testing分支了

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-10.39.28.15sxv2re3wxs.png"/>

此时，当你再修改并提交时：

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-10.41.14.70n8gzwgl4k0.png"/>

切换回 master 后：

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-10.42.06.5b0qqqtgfcw0.png"/>

**在切换分支时，一定要注意你工作目录里的文件会被改变。** 如果是切换到一个比较旧的分支，你的工作目录会恢复到该分支最后一次提交时的样子。如果 Git 不能干净利落地完成这个任务，它将禁止切换分支。

切换回 master 后再次对文件进行修改并提交：

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-10.45.45.gdbnqi4zmrk.png"/>

使用如下命令可以查看分支情况：

```powershell
$ git log --oneline --decorate --graph --all
```

由于 Git 的分支实质上仅是包含所指对象校验和的文件，所以它的创建和销毁都异常高效。

使用如下命令，创建分支的同时切换过去：

```powershell
$ git checkout -b <branchname>
```

### 四.分支的新建与合并

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-11.00.41.cjo5v5yf6m8.png"/>

两种情况：

1. 修复原 master 上的错误（hotfix）

修复完成后，切换到 master 分支，然后进行合并即可（fast-forward，此时只有一个父对象）

```powershell
$ git checkout master
$ git merge hotfix
```

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-11.04.41.6utsvwsojss0.png"/>

此时，hotfix 分支就不需要了，可以进行删除操作：

```powershell
$ git branch -d hotfix
```

2. 再修复完成原 master 上的错误后，此时已经出现了分叉，继续开发 issue53，然后进行合并（此时会拥有两个父对象）

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-11.10.04.1hgeawlbsnhc.png"/>

```powershell
$ git branch master
$ git merge iss53
```

此时，因为 master 分支所在提交并不是 iss53 分支所在提交的公共祖先，Git 不得不做一些额外的工作。出现这种情况的时候，Git 会使用两个分支的末端所指的快照（c4 和c5）以及这两个分支的公共祖先（c2），做一个简单的三方合并。

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-11.13.40.79gaf3oudt80.png"/>

和之前将分支指针向前推进所不同的是，Git 将此次三方合并的结果做了一个新的快照并且自动创建一个新的提交指向它。这个被称作一次合并提交，它的特别之处在于他不止一个父提交。

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-11.16.00.1aj6nn3wudc0.png"/>

最后将 iss53 分支删除即可：

```powershell
$ git branch -d iss53
```

### 五.分支管理

获取当前所有分支：

```powershell
$ git branch
```

如果分支前带有`*`则代表当前分支（即 HEAD 所指向的分支）。

查看每个分支的最后一次提交：

```powershell
$ git branch -v
```

查看哪些分支已经合并到当前分支：

```powershell
$ git branch --merged
```

查看哪些分支没有合并到当前分支：

```powershell
$ git branch --no-merged
```

这会显示未合并工作的分支尝试使用`git branch -d`命令删除会失败

如果想要强制删除，可以使用如下命令：

```powershell
$ git branch -D branchname
```

### 六.远程分支

远程分支是对远程仓库的引用（指针），包括分支，标签等等。

可以通过以下命令获取远程引用的完整列表：

```powershell
$ git ls-remote show <remote>
```

远程跟踪分支是远程分支状态的引用。它们是你无法移动的本地引用。一旦你进行了网络通信，Git 就会为你移动它们以反映远程仓库的状态。

它们以`<remote>/<branch>`的形式命名，例如要查看最后一次与远程仓库 origin 通信时master 分支的状态，你可以查看origin/master分支。

从远程服务器克隆，Git 的 clone 命令会为你自动将其命名为 origin，拉取它的所有数据，创建一个指向它的 master 分支的指针，并且在本地将其命名为 origin/master。Git 也会给你一个与 origin 的 master 分支在指向同一个地方的本地 master 分支，这样就有了工作的基础。

（**origin 并无特殊含义，是你在运行`git clone`时默认的远程仓库名字**，如果运行`git clone -o booyah`,那么你默认的远程分支名字将会是 booyah/master。同时 **master 是当你运行 `git init` 时默认的起始分支名字，也无特殊含义**）

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-11.56.03.3gqdajv8txk0.png"/>

如果你在本地的 master 分支做了一些工作，在同一段时间内有其他人推送提交到 [git.ourcompany.com](http://git.ourcompany.com/) 并且更新了它的 master 分支，这就是说你们的提交历史已走向不同的方向。 即便这样，只要你保持不与 origin 服务器连接(并拉取数据)，你的 origin/master 指针就不会移动。

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-11.57.13.4tk5cnoz2bk0.png"/>

如果要与远程服务器进行同步，运行`git fetch <remote>`（本例中为git fench origin）。这个命令查找‘origin’是哪一个服务器（git.ourcompany.com），从中抓取本地没有的数据，并且更新本地数据库，移动origin/master指针到更新之后的位置。

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-12.01.37.52cihhxb7c00.png"/>

你可以通过如下命令添加一个新的仓库：

```powershell
$ git remote add teamone git://git.team1.ourcompany.com
```

（其中 teamone 就是远程仓库的别名，像 origin 一样，后面是远程仓库的地址，以后就可以使用 teamone 来代替后面的地址）

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-12.04.28.1glyzm6v6b40.png"/>

使用`git fetch teamone`来抓取远程仓库 teamone 有而本地没有的数据。 

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-12.06.47.6ytgly9o4sw0.png"/>

（teamone 中的内容是 origin/master中的一个子集）

1. 推送

当你想要公开分享一个分支时，需要将其推送到有写入权限的仓库上。本地的分支并不会自动与远程仓库同步，你必须显式地推送想要分享的分支。

```powershell
$ git push <remote> <branch>
```

例如：

```powershell
$ git push origin master
```

也可以这样：

```powershell
$ git push origin serverfix:serverfix
```

也就是说推送本地的 serverfix 分支，将其作为远程仓库的 serverfix 分支。

如果并不想让远程仓库上的分支叫做 serverfix，也可以运行如下命令：

```powershell
$ git push origin serverfix:awesommebranch
```

将本地的 serverfix 分支推送到远程仓库上的 awesomebranch 分支

2. 跟踪分支

跟踪分支是与远程分支有直接关系的本地分支，如果在一个跟踪分支上输入`git pull`，Git 能自动识别去哪个服务器上抓取，合并到哪个分支。

当克隆一个仓库时，它通常会自动地创建一个跟踪`origin/master`的 master 分支。然而，如果你愿意的话，可以设置其他的跟踪分支，或是一个在其他远程仓库上的跟踪分支，又或者不跟踪 master 分支。

将当前分支跟踪到远程的 serverfix 分支的命令：

```powershell
$ git checkout --track origin/serverfix
```

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-12.35.28.3udswtc885g0.png"/>

3. 拉取

```powershell
$ git pull
```

该命令会查找当前分支所跟踪的服务器与分支，从服务器上抓取数据然后尝试合并入那个远程分支。

4. 删除远程分支

```powershell
$ git push origin --delete serverfix
```

（这个命令做的只是从服务器上移除这个指针）

### 七.变基

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-12.42.01.8qwkh9b7nqc.png"/>

通过命令：

```powershell
$ git checkout experiment
$ git rebase master
```

得到：

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-12.43.26.2v21igh0w080.png"/>

原理：首先找到这两个分支的最近共同祖先 C2，然后对比当前分支相对与该祖先的历次提交，提交相应的修改并存为临时文件，然后将当前分支指向目标基底C3，最后以此将之前另存为临时文件的修改依序应用。

最后，回到 master 分支上进行合并：

```powershell
$ git checkout master
$ git merge experiment
```

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240201/截屏2024-02-01-12.48.04.4roelfbt7vg0.png"/>

变基和第三方合并整合的最终结果所指向的快照始终是一样的，只不过提交历史不一样罢了。