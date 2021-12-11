# 1. Linux的文件权限

## 1.1 用户与用户组
在Linux里面，任何一个文件都具有用户（User）、所属群组（Group）及其他人（Others）三种身份的权限。
Linux系统中，账号与用户相关信息，记录在/etc/passwd这个文件内；密码记录在/etc/shadow文件内；组名记录在/etc/group文件内。

## 1.2 文件权限修改
chgrp（change group）：修改文件所属用户组
```shell
# -R：递归修改，连同子目录下的所有文件、目录都修改
chgrp [-R] 用户组名 dirname/filename
```

chown（chage owner）：修改文件拥有者
```shell
chown [-R] 用户名 文件或目录
chown [-R] 用户名:用户组名 文件或目录
```

chmod：修改文件的权限
```shell
chmod [-R] 777 文件或目录
```

## 1.3 权限对文件和目录的意义
权限对文件的意义：
1. r：可读取此文件的内容；
2. w：可以编辑该文件的内容；
3. x：可以执行该文件。

权限对目录的意义：
1. r：具有读取目录结构列表的权限；
2. w：建立新文件和目录的权限；删除已有的文件和目录的权限；修改已有文件和目录名的权限；移动已有文件和目录位置的权限；
3. x：用户可以进入该目录的权限。

## 1.4 文件种类
1. 常规文件：第一个字符为[-]
2. 目录：第一个字符为[d]
3. 链接文件：第一个字符为[l]
4. 设备与设备文件（与系统周边及存储等相关的一些文件，通常在/dec目录下），区块设备文件第一个字符为[b]；字符设备文件第一个字符为[c]
5. 数据接口文件：第一个字符为[s]
6. 数据输送文件：第一个字符为[p]

## 1.5 文件名
最大容许文件名为255字节，若以每个汉字占用2个字节来说，最大文件名就是大约128个汉字之间。


# 2. Linux目录结构
FHS针对目录树架构仅定义出三层目录：
1. /（root）：与启动系统有关；
2. /usr（unix software resource）：与软件安装/执行有关；
3. /var（variable）：与系统运行过程有关；

# 3. Linux的文件和目录管理

## 3.1 目录的相关操作
```shell
# 回到自己的家目录
cd ～
# 回到上层目录
cd ..
# 回到刚刚的那个目录
cd -
```

```shell
# 显示目前所在的目录
# -P：显示出真正的路径，而非使用链接的路径
pwd [-P]
```

```shell
# 创建目录，使用默认权限
mkdir test
# 创建目录，使用指定权限
mkdir -m 777 test
# 递归创建目录
mkdir -p test/test1/test2
```

## 3.2 执行文件路径的变量
```shell
# 显示当前用户的环境变量PATH，多个环境变量之间用(:)来隔开
echo $PATH
# 添加环境变量，把/root添加到环境变量中
PATH = "${PATH}:/root"
```

## 3.3 文件和目录管理
```shell
# 文件和目录的查看
ls
# -a：全部的文件，连同隐藏文件
# -d：仅列出目录本身，而不是列车目录内的文件
# -l：详细信息显示，包括文件的属性、权限等
```

```shell
# 复制文件和目录
cp [options] source destination
# -a：相当于-dr --preserve=all
# -i：若目标文件已经存在时，在覆盖时会先询问
# -d：如源文件为链接文件的属性，则复制链接文件属性而非文件本身
# -r：递归复制
# -p：连同文件的属性（权限、用户、时间）一起复制
```

```shell
# 删除文件和目录
rm [-fir] 文件或目录
# -f：删除时不询问
# -i：删除前询问是否删除
# -r：递归删除
```

```shell
# 移动文件和目录，重命名
mv [-fiu] source destination
# -f：移动时不询问
# -i：如果目标已存在该文件，则询问是否覆盖
# -u：如果目标已存在该文件，且源文件比较新，则覆盖
```

## 3.4 文件内容查看
```shell
# 直接显示文件内容
cat file
# -b：列出行号，仅针对非空白行显示行号
```

```shell
# 翻页查看
more file
# 在more这个程序运行过程中，可使用下面按键
# 空格键：向下翻一页
# Enter：向下翻一页
# /字符串：在显示的内容中，查询指定的字符串，按n可以继续匹配下一个
# q：退出
```

```shell
# 翻页查看
less file
# 在less这个程序运行过程中，可使用下面按键
# pagedown：向下翻一页
# pageup：向上翻一页
# /字符串：在显示的内容中，查询指定的字符串，按n可以继续匹配下一个
# g：前进到这个数据的第一行
# G：前进到这个数据的最后一行
# q：退出
```

```shell
# 数据截取，显示前n行数据，不指定-n则默认显示前10行
# 如果number为负数，则显示除尾部number行外的所有数据
head [-n number] file
```

```shell
# 数据截取，显示后n行数据，不指定-n则默认显示前后0行
# 如果number为负数，则显示除头部number行外的所有数据
tail [-n number] file
# -f：持续刷新显示后面所接文件中的内容，按ctrl+c结束
```

## 2.1 sql的编写过程

```sql
select distinct ... from ... join ... on ... where ... group by ... having ... order by ... limit ...
```

## 2.2 sql的解析过程

```sql
from ... on ... join ... where ... group by ... having ... select distinct ... order by ... limit ...
```

## 2.3 优化示例

```sql
CREATE TABLE `student` (
  `id` int(10) NOT NULL,
  `name` varchar(20) NOT NULL,
  `age` int(10) NOT NULL,
  `sex` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `student_union_index` (`name`,`age`,`sex`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

```sql
explain select name from student where age = 18 and sex in (0,1);
-- type类型是index
```

```sql
-- 删除旧索引
drop index student_union_index on student;
-- 创建新索引，修改顺序
alter table student add index student_union_index(age,sex,name);
explain select name from student where age = 18 and sex in (0,1);
-- type级别发生了变化，由index变为了range
```

```sql
-- 去掉in
-- in会导致索引失效，所以触发using where，进而导致回表查询
explain select name from student where age = 18 and sex = 1;
-- type级别发生了变化，由range变为了ref
```

## 2.4 优化总结

1. 保持索引的定义和使用顺序一致性；
2. 将含in的范围查询，放到where条件的最后，防止索引失效；

# 3. 两表优化

## 3.1 优化示例

```sql

CREATE TABLE `student` (
  `id` int(10) NOT NULL,
  `name` varchar(20) NOT NULL,
  `age` int(10) NOT NULL,
  `sex` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `teacher` (
  `id` int(11) DEFAULT NULL,
  `name` varchar(100) DEFAULT NULL,
  `course` varchar(100) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

```sql
-- 左连接，联合查询时，小表驱动大表
explain select teacher.name,student.name from teacher left join student on teacher.id = student.id  where teacher.course = '数学'
-- teacher表type级别为ALL
-- student表type级别为ALL
```

```sql
-- left join，一般给左表加索引，因为是驱动表
alter table teacher add index teacher_index(id);
alter table teacher add index teacher_course(course);
explain select teacher.name,student.name from teacher left join student on teacher.id = student.id  where teacher.course = '数学'
-- teacher表type级别为ref
-- student表type级别为ALL
```

## 3.2 优化总结

1. 小表驱动大表
2. 索引建立在经常查询的字段上
3. sql优化，是一种概率层面的优化，是否实际使用了我们的优化，需要通过explain推测

# 4. 避免索引失效

1、复合索引，不要跨列或无序使用（最佳左前缀）；

2、复合索引，尽量使用全索引匹配；

3、不要在索引上进行任何操作，例如对索引进行（计算、函数、类型转换）；

4、复合索引不能使用不等于（!= 或 <>）或 is null（is not null）；

5、尽量使用覆盖索引（using index）；

6、like尽量以常量开头，不要以%开头，否则索引失效；如果必须使用%name%进行查询，可以使用覆盖索引挽救，不用回表查询时可以触发索引；

7、尽量不要使用类型转换，否则索引失效；

8、尽量不要使用or，否则索引失效；

# 5. 其他优化方式

## 5.1 exist和in的选择

```sql
select name,age from student exist/in (子查询);

-- 如果主查询的数据集大，则使用in；
-- 如果子查询的数据集大，则使用exist；
```

## 5.2 order by的优化

using filesort有两种算法：

1. 双路排序（根据IO的次数）：扫描两次磁盘（①从磁盘读取排序字段，对排序字段进行排序；②获取其它字段）；

2. 单路排序：只读取一次（全部字段），在buffer中进行排序。但单路排序会有一定的隐患（不一定真的是只有一次IO，有可能多次IO）。

```sql
-- 单路排序会比双路排序占用更多的buffer，如果数据量较大，可以调大buffer的容量大小
set max_length_for_sort_data = 1024;单位是字节byte。
```

MySQL4.1之前，默认使用双路排序；MySQL4.1之后，默认使用单路排序；如果max_length_for_sort_data值太低，MySQL底层会自动将单路切换到双路。 

提高order by查询的策略：

1. 选择使用单路或双路，调整buffer的容量大小；
2. 避免select * from student;（① MySQL底层需要对*进行翻译，消耗性能；② *永远不会触发索引覆盖 using index）；
3. 复合索引不要跨列使用，避免using filesort；
4. 保证全部的排序字段，排序的一致性（都是升序或降序）；

# 6. 慢查询日志

慢查询日志就是MySQL提供的一种日志记录，用于记录MySQL响应时间超过阈值的SQL语句（long_query_time，默认10秒） ；慢查询日志默认是关闭的，开发调优时打开，最终部署时关闭。

## 6.1 开启慢查询日志

```sql
-- 查询慢查询日志是否开始
mysql> show variables like '%slow_query_log%';
+---------------------+-----------------------------+
| Variable_name       | Value                       |
+---------------------+-----------------------------+
| slow_query_log      | OFF                         |
| slow_query_log_file | /var/lib/mysql/192-slow.log |
+---------------------+-----------------------------+
```

```sql
-- 临时开启
set global slow_query_log = 1;

-- 永久开启，在etc/my.cnf中添加配置
slow_query_log=1
slow_query_log_file=/var/lib/mysql/localhost-slow.log
```

```sql
-- 重启数据库
service mysql restart;
```

## 6.2 设置慢查询阈值

```sql
-- 查询慢查询阈值
mysql> show variables like '%long_query_time%';
+-----------------+-----------+
| Variable_name   | Value     |
+-----------------+-----------+
| long_query_time | 10.000000 |
+-----------------+-----------+
```

```sql
-- 临时修改阈值
set global long_query_time = 5;

-- 永久修改阈值，在etc/my.cnf中添加配置
long_query_time = 5;
```

## 6.3 查看慢查询的数量

```sql
show global status like '%slow_queries%';
```

## 6.4 mysqldumpslow工具

通过mysqldumpslow工具查看慢sql，可以通过一些过滤条件，快速查出需要定位的慢sql。 

```shell
mysqldumpslow [ OPTS... ] [ LOGS... ]
-s ORDER     what to sort by (al, at, ar, c, l, r, t), 'at' is default
              al: average lock time
              ar: average rows sent
              at: average query time
               c: count
               l: lock time
               r: rows sent
               t: query time 
-r           reverse the sort order
-t NUM       just show the top n queries
-g PATTERN   grep: only consider stmts that include this string
-h HOSTNAME  hostname of db server for *-slow.log filename (can be wildcard),
               default is '*', i.e. match all
-i NAME      name of server instance (if using mysql.server startup script)
-l           don't subtract lock time from total time
```

# 7. 锁机制

## 7.1 操作分类

读写：对同一个数据，多个读操作可以同时进行，互不干扰。

写锁：如果当前写操作没有完毕，则无法进行其它的读写操作。

## 7.2 操作范围

表锁：一次性对一张表整体加锁。

如MyISAM存储引擎使用表锁，开销小、加锁快、无死锁；但锁的范围大，容易发生冲突、并发度低。

行锁：一次性对一条数据加锁。

如InnoDB存储引擎使用的就是行锁，开销大、加锁慢、容易出现死锁；锁的范围较小，不易发生锁冲突，并发度高.

## 7.3 加锁和查看加锁

```sql
-- 加锁
lock table 表1 read/write，表2 read/write，...
-- 查看加锁的表
show open tables;
```

## 7.4 MyISAM表级锁的锁模式

MyISAM在执行查询语句前，会自动给涉及的所有表加读锁，在执行增删改前，会自动给涉及的表加写锁。所以对MyISAM表进行操作，会有如下情况发生：

1. 对MyISAM表的读操作（加读锁），不会阻塞其它会话（进程）对同一表的读请求。但会阻塞对同一表的写操作。只有当读锁释放后，才会执行其它进程的写操作。
2. 对MyISAM表的写操作（加写锁），会阻塞其它会话（进程）对同一表的读和写操作，只有当写锁释放后，才会执行其它进程的读写操作。

## 7.5 InnoDB分析表锁定

```sql
mysql> show status like '%innodb_row_lock%';
+-------------------------------+-------+
| Variable_name                 | Value |
+-------------------------------+-------+
| Innodb_row_lock_current_waits | 0     |
| Innodb_row_lock_time          | 0     |
| Innodb_row_lock_time_avg      | 0     |
| Innodb_row_lock_time_max      | 0     |
| Innodb_row_lock_waits         | 0     |
+-------------------------------+-------+

-- Innodb_row_lock_current_waits：当前正在等待锁的数量
-- Innodb_row_lock_time：等待总时长。从系统启动到现在一共等待的时间
-- Innodb_row_lock_time_avg：平均等待时长。从系统启动到现在一共等待的时间
-- Innodb_row_lock_time_max：最大等待时长。从系统启动到现在一共等待的时间
-- Innodb_row_lock_waits：等待次数。从系统启动到现在一共等待的时间
```

