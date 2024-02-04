---
title: Git常用命令记录
tags: ["Git"]
image: /static/images/git.png
pubDate: 2024-2-4
featured: false
---

1. **删除分支**

   删除本地分支：

   ```powershell
   git branch -d <branch-name>
   ```

   强制删除本地分支：

   ```powershell
   git branch -D <branch-name>
   ```

   删除远程分支：

   ```powershell
   git push --delete <remote-name> <branch-name>
   ```

   

2. **本地分支推送到远程分支**

   远程还未创建该分支：

   ```powershell
   git push -u origin <branch-name>  //-u是将本地分支与远程分支跟踪
   ```

   若是已经跟踪，则只需切换到相关分支，然后直接推送：

   ```powershell
   git switch <branch-name>
   git push
   ```

   

3. **创建一个新分支**

   ```powershell
   git branch <branch-name>
   ```

   

4. **显示所有分支**

   ```powershell
   git branch
   ```



5. **添加远程仓库**

   ```powershell
   git remote add origin <ssh-url>
   ```

   

6. **显示远程仓库**

   ```powershell
   git remote
   git remote -v //显示详细信息
   ```

   

7. **删除远程仓库引用**

   ```powershell
   git remote remove <reomote-name>
   git remote remove origin
   ```

   

8. **远程仓库修改名称后，更新本地引用**

   ```powershell
   git remote set-url <remote-name> <ssh-url>
   git remote set-url origin <ssh-url>
   ```

   

9. **一次性提交**

   ```powershell
   git commit -am "change sth"
   ```

   <mark>需要注意，如果是新建的文件需要先 git add .跟踪之后才能使用一次性提交</mark>

10. **查看仓库状态**

   ```powershell
   git status
   ```

   

11. **git diff 的使用**

    查看工作区与暂存区的差异：

    ```powershell
    git diff
    ```

    查看暂存区与上次提交的差异：

    ```powershell
    git diff --staged
    git diff --cached
    ```

    查看两次提交之间的差异：

    ```powershell
    git diff commit1-hash commit2-hash //使用哈希值
    ```

    查看单独一个文件的修改：

    ```powershell
    git diff <file-name>
    ```

    查看不同分支最新提交之间的差异：

    ```powershell
    git diff <branch1-name> <branch2-name>
    ```

    

12. **git reset的使用**

    回退的同时删除工作区和暂存区：

    ```powershell
    git reset --hard <commit-hash>
    ```

    回退的同时只删除暂存区：

    ```powershell
    git reset --mixed <commit-hash>
    ```

    只回退，保留工作区和暂存区的内容；

    ```powershell
    git reset --soft <commit-hash>
    ```

    使用 git reset后，需要强制覆盖远程仓库的历史：

    ```powershell
    git push --force
    ```

    

13. **git log 和 git reflog 的使用与区别**

    显示历史提交信息（commit）：

    ``` powershell
    git log
    git log --oneline
    ```

    <img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240204/截屏2024-02-04-21.56.41.1pk59etdblds.png" alt="截屏2024-02-04-21" />

    显示本地仓库中 HEAD 和分支指针的变化历史，提交、合并、重置和分支的检出等：

    ```powershell
    git reflog
    git reflog --oneline
    ```

    <img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240204/截屏2024-02-04-21.57.23.32u2ng8nyyc0.png" alt="截屏2024-02-04-21" />

    - **恢复能力**：`git reflog` 可以帮助你恢复到几乎任何之前的状态，即使那些状态已经被删除或丢弃，只要它们还在本地仓库的操作历史中。
    - **适用范围**：`git log` 的信息是存储在项目的历史中，可以被推送到远程仓库；而 `git reflog` 的信息是本地的，每个克隆的仓库都有自己的 reflog，不会被推送到远程仓库。



12. 从远程拉取并合并

    拉取：

    ```powershell
    git fetch origin
    ```

    比较远程与本地之间的差异：

    ```powershell
    git diff main origin/main
    ```

    合并到本地：

    ```powershell
    git merge <remote-name>/<branch-name>
    git merge origin/main
    ```

    
