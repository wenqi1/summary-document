```shell
# 1.搜索镜像
docker search tomcat
# 2.拉取镜像
docker pull tomcat[:tag]
# 3.运行镜像
docker run -d -p 8080:8080 --name=tomcat1 tomcat
# 新版tomcat的webapp下为空，访问8080会报404，现在放在了webapps.dist，解决如下
# 4.交互式方式进入容器
docker exec -it 容器id /bin/bash
# 5.进入tomcat的根目录
cd /usr/local/tomcat
# 5.删除webapps目录
rm -f webapps
# 6.将webapps.dist改名为webapps
mv webapps.dist webapps
```