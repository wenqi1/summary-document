# 1. git init命令
```shell
# 在目录中创建新的Git仓库
git init
```
# 2. git clone命令
```shell
# 拷贝一个Git仓库到本地
git clone [url]
```

# 3. git add命令
```shell
# 添加一个或多个文件到暂存区
git add [file1] [file2]...

# 添加指定目录到暂存区
git add [dir]

# 添加当前目录下的所有文件到暂存区
git add .

```

# 4. git status命令
```shell
# 查看上次提交之后是否对文件进行修改
# -s：简化输出结果（"表示未修改；M表示修改；A表示已添加；D表示删除；R表示重命名；C表示复制；U表示更新未被合并）
git status [-s]
```

# 5. git commit命令
```shell
# 将指定文件添加到本地仓库
git commit [file1] [file2]... [-m message]
# 将暂存区内容添加到本地仓库
git commit [-m message]
```

# 6. git diff命令
```shell
# 显示暂存区和工作区的差异
git diff [file]
# 显示暂存区和上一次提交的差异
git diff -cached [file]
```

# 7. git reset命令
```shell
# HEAD：表示当前版本(等价于HEAD~0)
# HEAD^：表示上一个版本（等价于HEAD~1）
# HEAD^^：表示上上个版本(等价于HEAD~2)
# 以此类推
git reset [--soft | --mixed | hard] [HEAD]
```
--soft：仅将HEAD指针指向历史版本
--mixed：将HEAD指针指向历史版本，且用历史版本覆盖当前的暂存区
--hard：将HEAD指针指向历史版本，且用历史版本覆盖暂存区和工作区

# 8. git rm命令
```shell
# 将文件从暂存区中删除
git rm --cached [file]
# 将文件从暂存区和工作区中删除
git rm [file]
# 删除修改过且已经放到暂存区的文件，需要添加-f强制删除
git rm -f [file]
# 将一个目录递归删除
git rm -r [dir]
```

# 9. git mv命令
```shell
# 重命名一个文件或目录
git mv [file] [newfile]
```

# 10. git log命令
```shell
# 查看历史提交记录
git log
# 查看简洁历史提交记录
git log --oneline
# 查看带分支、合并等信息的历史提交记录
git log --graph
# 查看指定作者提交的历史记录
git log --author=wenqi1
# 查看指定时间段提交的历史记录
git log --before={3.weeks.ago} --after={2021-12-11}
```

# 11. git blame命令
```shell
# 查看指定文件的修改记录
git blame <file>
```

# 12. git remote命令
```shell
# 显示所有远程仓
git remote -v
# 显示指定远程仓的信息
git remote show [remote url]
# 添加远程仓库
git remote add [仓库名] [url]
# 删除远程仓库
git remote rm [仓库名]
# 修改仓库名
git remote rename [旧名][新名]
```

# 13. git fetch命令
```shell
# 从远程获取代码库
git fetch [仓库名]
```

# 14. git pull命令
```shell
# 从远程获取代码并合并本地，相当于git fetch和git merge
git pull <仓库名> <分支名>
```

# 15. git push命令
```shell
# 将本地的分支版本上传到远程并合并
git push <仓库名> <分支名>
# 如果本地与远程有差异，想强制推送
git push --force <仓库名> <分支名>
```

# 16. git branch/merge命令
```shell
# 创建新分支
git branch <branchname>
# 创建新分支，并切换到新分支
git branch -b <branchname>
# 切换分支
git checkout <branchname>
# 删除分支
git branch -d <branchname>
# 将指定分支合并到当前分支
# 合并时如果产生了冲突，需要编辑冲突的文件
# 编辑完成后，git add <冲突文件>，告诉Git文件冲突已解决
# 冲突解决后，git commit提交
git merge <branchname>
```

# 17. git tag命令
```shell
# 打标签
git tag -a <tagname> -m "标签注释"
# 给历史提交打标签
git tag -a <tagname> <提交记录id> -m "标签注释"
# 提交单个tag到远程仓
git push <仓库名> <tagname>
# 提交所有tag到远程仓
git push <仓库名> --tags
```