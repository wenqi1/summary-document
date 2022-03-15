# Redis单机版
```shell
# 1.搜索镜像
docker search redis
```
```shell
# 2.拉取镜像
docker pull redis[:tag]
```
```shell
# 3.运行镜像
docker run -d -p 6379:6379 -privileged=true -v /users/wenqi/redis/redis.conf:/etc/redis/redis.conf -v /users/wenqi/redis/data:/data --name redis redis:tag redis-server /etc/redis/redis.conf
```
```shell
# 4.修改redis.conf
vi /users/wenqi/redis/redis.conf
开启密码验证：requirepass 123456
开启远程连接注释掉： bind 127.0.0.1
开始后台启动：daemonize yes
开启持久化：appendonly yes
```
```shell
# 5.重启redis容器
docker restart redis
```

# Redis集群版（三主三从）
```shell
# 1.启动6台Redis
docker run -d --net host --privileged=true -v /users/wenqi/redis/redis-node-1/data:/data --name redis-node-1 redis:tag --cluster-enabled yes --appendonly yes --port 6381

docker run -d --net host --privileged=true -v /users/wenqi/redis/redis-node-2/data:/data --name redis-node-2 redis:tag --cluster-enabled yes --appendonly yes --port 6382

docker run -d --net host --privileged=true -v /users/wenqi/redis/redis-node-3/data:/data --name redis-node-3 redis:tag --cluster-enabled yes --appendonly yes --port 6383

docker run -d --net host --privileged=true -v /users/wenqi/redis/redis-node-4/data:/data --name redis-node-4 redis:tag --cluster-enabled yes --appendonly yes --port 6384

docker run -d --net host --privileged=true -v /users/wenqi/redis/redis-node-5/data:/data --name redis-node-5 redis:tag --cluster-enabled yes --appendonly yes --port 6385

docker run -d --net host --privileged=true -v /users/wenqi/redis/redis-node-6/data:/data --name redis-node-6 redis:tag --cluster-enabled yes --appendonly yes --port 6386
```

```shell
# 2.构建主从实例关系
# --cluster-replicas 1 代表master:slave = 1:1
docker exec -it redis-node-1 /bin/bash
redis-cli --cluster create ip:6381 ip:6382 ip:6383 ip:6384 ip:6385 ip:6386 --cluster-replicas 1
```

```shell
# 3.查看集群相关信息
redid-cli -p 6381
# 查看集群信息
cluster info
# 查看集群节点
cluster node
```

```shell
# 4.集群检查
redis-cli --cluster check ip:port
```

```shell
# 集群方式连接
redis-cli -p 6381 -c
```

# 集群扩容（四主四从）
```shell
# 1.新启动2台Redis
docker run -d --net host --privileged=true -v /users/wenqi/redis/redis-node-1/data:/data --name redis-node-7 redis:tag --cluster-enabled yes --appendonly yes --port 6387

docker run -d --net host --privileged=true -v /users/wenqi/redis/redis-node-2/data:/data --name redis-node-8 redis:tag --cluster-enabled yes --appendonly yes --port 6388
```

```shell
# 2.将redis-node-7加入集群
docker exec -it redis-node-7 /bin/bash
redis-cli --cluster add-node ip:6387 ip:6381
```

```shell
# 3.给新加入的分配槽位
# 4个master平均分配16384槽，每个master4096
# 新加入的master槽位是之前3个master匀给它的
redis-cli --cluster reshard ip:6381
```

```shell
# 4.给新master分配slave
# 主节点id通过edis-cli --cluster check ip:port查询
redis-cli --cluster add-node ip:6388 ip:6387 --cluster-slave --cluster-master-id 主节点id
```

# 集群缩容（三主三从）
```shell
# 1. 查询6388节点的id
docker exec -it redis-node-1 /bin/bash
redis-cli --cluster check ip:port
```

```shell
# 2. 将集群中的6388删除
redis-cli --cluster del-node ip:6388 6388节点id
```

```shell
# 3. 将6387节点的槽位清空，重新分配，槽位全部给6381
redis-cli --cluster reshard ip:6381
```

```shell
# 4. 将集群中的6387删除
redis-cli --cluster del-node ip:6387 6387节点id
```