# 1. Docker三要素
1. 镜像（image）
docker image是一个只读的模板，image用来创建docker容器，一个镜像可以创建多个容器。
2. 容器（container）
docker利用容器独立运行的一个或一组应用。容器就是用镜像创建的运行实例。
3. 仓库（repository）
仓库是集中存放镜像文件的场所。

# 2. 镜像相关命令
```shell
# 列出本地主机上的镜像
docker images [options]
-a：显示所有镜像
-q：只显示镜像id
```

```shell
# 查询镜像
docker search [options] 镜像名
--limit：指定显示多少条数据
```

```shell
# 拉取镜像
docker pull 镜像名[:TAG]
```

```shell
# 查看镜像、容器、数据卷所占的空间
docker system df
```

```shell
# 删除镜像
docker rmi 镜像名/镜像ID
-f：强制删除
# 删除全部镜像
docker rmi -f $(docker images -aq)
```

# 3. 容器相关命令
```shell
# 启动新容器
docker run [options] 镜像名 [commond][arg...]
-d：后台运行容器并返回容器id
-i：以交互模式运行容器
-t：为容器分配一个伪输入终端
-name：指定容器的名字
-p：指定端口映射
```

```shell
# 查看正在运行的容器
docker ps [options]
-a：显示所有容器（正在运行和停止运行的）
-q：只显示容器id
```

```shell
# 启动停止的容器
docker start 容器id/容器名
```

```shell
# 停止容器
docker stop 容器id/容器名
# 强制停止容器
docker kill 容器id/容器名
```

```shell
# 重启容器
docker restart 容器id/容器名
```

```shell
# 删除容器
docker rm -f 容器id
# 删除所有容器
docker rm -f $(docker ps -aq)
```

```shell
# 查看容器日志
docker logs 容器id
```

```shell
# 查看容器内运行的进程
docker top 容器id
```

```shell
# 查看容器内部细节
docker inspect 容器id
```

```shell
# 重新以交互方式进入容器（exit退出不会导致容器停止）
docker exec -it 容器id /bin/bash
# 重新以交互方式进入容器（exit退出会导致容器停止）
docker attach 容器id
```

```shell
# 拷贝容器内的数据到主机
docker cp 容器id:容器内路径 主机路径
# 导出容器为tar包
docker export 容器id > 文件名.tar
# 导入tar包为镜像
cat 文件名.tar ｜ docker import - 镜像用户/镜像名:镜像版本号
```

```shell
# 提交容器副本使之成为一个新的镜像
docker commit -m="提交的描述信息" -a="作者" 容器id 新镜像名[:版本号]
```

# 4. 构建私有仓库
```shell
# 1.下载镜像Docker Registry
docker pull Registry
# 2.运行Registry
docker run -d -p 5000:5000 - v /wenqi/dockerregistry/:tmp/registry --privileged=true registry
# 3.上传到私有仓库的镜像tag需要符合标准（ip:port/镜像名:版本号）,ip为私有仓库ip，port为私有仓库的port
docker tag test:1.1 ip:port/test:1.1
# 4.上传镜像到私有仓库
docker push ip:port/test:1.1
```

# 5. 容器数据卷
```shell
# 将容器里的目录与主机的目录映射，数据互通
docker run -it -v 主机目录:容器目录 --privileged=true --name=ubuntu ubuntu
# 添加ro，表示容器里该目录下只能读
docker run -it -v 主机目录:容器目录:ro --privileged=true --name=ubuntu ubuntu
# 容器间数据卷的继承（容器ubuntu2继承容器ubuntu的数据卷映射关系）
docker run -it -volumns-from ubuntu --privileged=true --name=ubuntu2 ubuntu
```

# 6. Dockerfile
## 6.1 基础知识
1. 每条指令都必须为大写；
2. 指令安照从上到下顺序执行；
3. 每条指令都会创建一个新的镜像层并对镜像进行提交。
## 6.2 指令介绍
FROM：基础镜像
MAINTAINER：镜像维护者的姓名和邮箱地址
RUN：构建时需要运行的命令
EXPOSE：暴露端口
WORKDIR：进入交互模式时的当前路径
USER：执行镜像的用户，默认是root
ENV：构建时设置的环境变了
ADD：将主机目录下的文件拷贝进镜像且自动处理url和解压tar包
COPY：将主机目录下的文件拷贝到镜像中
VOLUMN：容器数据卷
CMD：容器启动后执行（多个CMD只有最会一个生效；会被docker run后面的命令覆盖）
ENTRYPOINT：容器启动后执行（不会被docker run后面的命令覆盖，会成为它的参数；如果CMD一起使用，CMD会成为它的参数）
## 6.3 构建镜像
```shell
docker build -t 镜像名:tag
```

# 7. Docker网络

