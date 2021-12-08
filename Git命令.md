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