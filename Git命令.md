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