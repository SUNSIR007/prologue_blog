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
   git push --delete <repo-name> <branch-name>
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

   

7. **一次性提交**

   ```powershell
   git commit -am "change sth"
   ```

   

8. **查看仓库状态**

   ```powershell
   git status
   ```

   

9. **git diff 的使用**

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

   

10. **git reset的使用**

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

    

    ---未完待续---