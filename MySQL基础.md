# 1. MySQL数据库简介

MySQL 是一个关系型数据库管理系统，由瑞典 MySQL AB 公司开发，目前属于 Oracle 公司。MySQL 是一种关联数据库管理系统，关联数据库将数据保存在不同的表中，而不是将所有数据放在一个大仓库内，这样就增加了速度并提高了灵活性。 

## 1.1 特点

1. MySQL 是开源的，目前隶属于 Oracle 旗下产品。
2. MySQL 支持大型的数据库。可以处理拥有上千万条记录的大型数据库。
3. MySQL 使用标准的 SQL 数据语言形式。
4. MySQL 可以运行于多个系统上，并且支持多种语言。这些编程语言包括 C、C++、Python、Java、Perl、PHP、Eiffel、Ruby 和 Tcl 等。
5. MySQL 支持大型数据库，支持 5000 万条记录的数据仓库，32 位系统表文件最大可支持 4GB，64 位系统支持最大的表文件为8TB。
6. MySQL 是可以定制的，采用了 GPL 协议，你可以修改源码来开发自己的 MySQL 系统。

## 1.2 数据库表的存储位置

MySQL数据表以文件方式存放在磁盘中：

1. 包括表文件、数据文件以及数据库的选项文件。
2. 位置：MySQL安装目录\data下存放数据表，目录名对应数据库名，该目录下文件名对应数据表。

# 2. 数据类型

## 2.1 整数类型

整数类型的数，默认情况下既可以表示正整数又可以表示负整数（此时称为有符号数）。如果只希望表示零和正整数，可以使用无符号关键字 unsigned 对整数类型进行修饰。

| 整数类型  |           含义            |
| :-------: | :-----------------------: |
|  tinyint  |   占1个字节（-128~127）   |
| smallint  | 占2个字节（-2^15~2^15-1） |
| mediumint | 占3个字节（-2^23~2^23-1） |
|    int    | 占4个字节（-2^31~2^31-1） |
|  bigint   | 占8个字节（-2^63~2^63-1） |

## 2.2 小数类型

|   小数类型   |             含义              |
| :----------: | :---------------------------: |
|  float(m,d)  |           占4个字节           |
| double(m,d)  |           占8个字节           |
| decimal(m,d) | decimal是存储为字符串的浮点数 |

## 2.3 字符串类型

| 字符串类型 |            含义            |
| :--------: | :------------------------: |
|  char(n)   |  固定长度，最多255个字符   |
| varchar(n) | 可变长度，最多65535个字符  |
|  tinytext  |  可变长度，最多255个字符   |
|    text    | 可变长度，最多65535个字符  |
| mediumtext | 可变长度，最多2^24-1个字符 |
|  longtext  | 可变长度，最多2^32-1个字符 |

## 2.4 日期类型

| 日期类型  | 字节数 |                取值范围                 |        零值         |
| :-------: | :----: | :-------------------------------------: | :-----------------: |
|   year    |   1    |                1901~2155                |        0000         |
|   time    |   3    |          -838:59:59~838:59:59           |      00:00:00       |
|   date    |   4    |          1000-01-01~9999-12-31          |     0000:00:00      |
| datetime  |   8    | 1000-01-01 00:00:00~9999-12-31 23:59:59 | 0000:00:00 00:00:00 |
| timestamp |   4    |      19700101080001~20380119111407      |   00000000000000    |

## 2.5 复合类型

MySQL 支持两种复合数据类型：

1. enum类型的字段类似于单选按钮的功能，一个enum类型的数据最多可以包含65535个元素。 
2. set 类型的字段类似于复选框的功能，一个set类型的数据最多可以包含64个元素。

## 2.6 二进制类型

二进制类型的主要用于存储由0和1组成的字符串，因此从某种意义上，二进制类型的数据是一种特殊格式的字符串。二进制类型与字符串类型的区别在于：字符串类型的数据按字符为单位进行存储，因此存在多种字符集、多种字符序；而二进制类型的数据按字节为单位进行存储，仅存在二进制字符集binary。

| 二进制类型   | 含义                                |
| ------------ | ----------------------------------- |
| binary(m)    | 固定长度，字节数为m，m最大值为255   |
| varbinary(m) | 可变长度，字节数为m，m最大值为65535 |
| bit(m)       | 字节数为m，m最大值为64              |
| tinyblob     | 可变长度，最多255个字节             |
| blob         | 可变长度，最多2^16-1个字节          |
| mediumblob   | 可变长度，最多2^24-1个字节          |
| longblob     | 可变长度，最多2^32-1个字节          |

# 3. 数据库约束

约束是一种限制，它通过对表的行或列的数据做出限制，来确保表的数据的完整性、唯一性。 

## 3.1 非空约束（not null）

- 非空约束用于确保当前列的值不为空值，非空约束只能出现在表对象的列上。
- null类型特征：所有的类型的值都可以是null，包括int、float 等数据类型

## 3.2 唯一性约束（unique）

- 唯一约束是指定表的列或列组合不能重复，保证数据的唯一性。
- 唯一约束不允许出现重复的值，但是可以为多个null。
- 同一个表可以有多个唯一约束，多个列组合的约束。
- 在创建唯一约束时，如果不给唯一约束名称，就默认和列名相同。
- 唯一约束不仅可以在一个表内创建，而且可以同时多表创建组合唯一约束。

## 3.3 主键约束（primary key）

- 主键约束相当于唯一约束 + 非空约束。
- 每个表最多只允许一个主键，建立主键约束可以在列级别创建，也可以在表级别创建。

## 3.4 外键约束（foreign key）

- 外键约束是用来加强两个表（主表和从表）的一列或多列数据之间的连接的，可以保证一个或两个表之间的参照完整性。
- 创建外键约束的顺序是先定义主表的主键，然后定义从表的外键。

## 3.5 默认值约束（default）

- 在表中定义了默认值约束，插入新的数据行时，如果该行没有指定数据，那么系统将默认值赋给该列，如果不设置默认值，系统默认为NULL。

## 3.6 自增长约束（auto_increment）

- 自增长约束(AUTO_INCREMENT)可以约束任何一个字段。
- 当插入第一条记录时，不指定自增字段，该自增字段就是从1开始，每插入一条记录，该自增字段的值加1。
- 当插入第一条记录时，给自增字段一个具体值，以后每插入一条记录就在该值的基础上加1。

# 4. 数据库常用命令

```sql
-- 启动服务
net start mysql

-- 停止服务
new stop mysql

--连接数据库
mysql -u 用户名 -p 密码;

-- 退出数据库
quit
```

# 5. DDL语句

DDL（Data Definition Languages）语句：即数据库定义语句

对于数据库而言每一张表都表示一个数据库的对象，而数据库对象指的就是DDL定义的所有操作，而对象的操作主要分为以下三类语法 ：

- 创建对象：CREATE 对象名称;
- 删除对象：DROP 对象名称;
- 修改对象：ALTER 对象名称;

## 5.1 数据库操作

```sql
-- 创建数据库
create database 库名;

-- 显示所有数据库
show databases;

-- 选择数据库
use 库名;

-- 删除数据库
drop database 库名;
```

## 5.2 表操作

```sql
-- 创建表
create table 表名(
	列名 类型,
	列名 类型,
	...
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- 查看所有引擎
show engines;

-- 查看默认引擎
show variables like 'deafult_storage_engine';

-- 查看表定义
desc 表名;

-- 查看创表语句
show create table 表名;

-- 删除表
drop table 表名;

-- 修改表字段类型
alter table 表名 modify 列名 新类型;

-- 添加表字段
alter table 表名 add column 列名 类型;

-- 添加表字段到指定字段后面
alter table 表名 add 列名 类型 after 列名;

-- 删除表字段
alter table 表名 drop column 列名;

-- 修改字段名
alter table 表名 change 列名 新列名;

-- 修改表名
alter table 表名 rename 新表名;

```

# 6. DML语句

DML（Data Manipulation Language）语句：即数据操纵语句

```sql
-- 插入一条数据
insert into 表名(列名1,列名2,...) values(值1,值2,...);

-- 插入多条数据
insert into 表名(列名1,列名2,...) values(值1,值2,...),(值1,值2,...);

-- 表数据复制
insert into 表名1 select * from 表名2;

-- 更新数据
update 表名 set 列名1 = 值 where 列名2 = 值

-- case/when 批量修改
update 表名 set 列名1 = case 列名2 
	when 值1 then '值4' 
	when 值2 then '值5' 
	when 值3 then '值6' 
end where 列名2 in (值1,值2,值3)

-- 删除数据
delete from 表名 where 列名 = 值;

-- 删除表里的所有数据
truncate 表名;
```

# 7. DQL语句

DQL（Data Query Language）语句：即数据查询语句

## 7.1 where条件语句

```sql
-- 根据条件查询数据库
select 列名 from 表名 where 列名1 = 值
```

### 7.1 .1 算数运算符

| 算数运算符 |   含义   |
| :--------: | :------: |
|     =      |   等于   |
|  <> 或 !=  |  不等于  |
|     >      |   大于   |
|     <      |   小于   |
|     >=     | 大于等于 |
|     <=     | 小于等于 |

### 7.1.2  逻辑运算符

| 逻辑运算符 | 含义 |
| :--------: | :--: |
| and 或 &&  |  与  |
| or 或 \|\| |  或  |
| not 或 ！  |  非  |

### 7.1.3  比较运算符

| 比较运算符  |        含义        |
| :---------: | :----------------: |
|   is null   |     是否为null     |
| is not null |    是否不为null    |
| between and |  是否在两个数之间  |
|    like     |      模糊匹配      |
|     in      | 是否在一个集合中间 |
|    exits    | 是否在一个集合中间 |

## 7.2 as取别名

```sql
select 列名 as 新列名 from 表名 where 列名1 = 值
```

## 7.3 distinct去重复

```sql
select distinct 列名 from 表名 where 列名1 = 值
```

## 7.4 group by分组

```sql
-- 单个列分组
select 列名 from 表名 group by 列名1;

-- 多个列分组
select 列名 from 表名 group by 列名1, 列名2;
```

## 7.5 having过滤

WHERE 搜索条件在进行分组操作之前应用，而HAVING 搜索条件在进行分组操作之后应用。HAVING 语法与 WHERE 语法类似，但 HAVING 可以包含聚合函数。HAVING 子句可以引用选择列表中显示的任意项。

```sql
select 列名 from 表名 group by 列名1 having count(*) > 2;
```

## 7.6 order by排序

```sql
-- 降序
select 列名 from 表名 order by 列名1 desc;

-- 升序
select 列名 from 表名 order by 列名1 asc;

-- 多列排序，第一个相同就按照第二个排序
select 列名 from 表名 order by 列名1 desc, 列名2 asc;
```

## 7.7 limit分页

```sql
select 列名 from 表名 limit 个数;
select 列名 from 表名 limit 起始位置 个数;
```

## 7.8 多表查询

### 7.8.1 笛卡尔积查询

笛卡尔积在SQL中的实现方式即交叉连接，两个表中的每一行数据任意组合。

```sql
select 列名 from 表名1, 表名2;
```

### 7.8.2 内连接

内连接可以看作是先对两个表进行交叉连接，再通过on剔除不符合条件的。

```sql
select 列名 from 表名1 inner join 表名2 on 表名1.id = 表名2.id;
```

### 7.8.3 左外连接

左边的表无论是否能够匹配都要完整显示，右边的仅展示匹配上的记录。

```sql
select 列名 from 表名1 left join 表名2 on 表名1.id = 表名2.id;
```

### 7.8.4 右外连接

右边的表无论是否能够匹配都要完整显示，左边的仅展示匹配上的记录。

```sql
select 列名 from 表名1 right join 表名2 on 表名1.id = 表名2.id;
```

# 8. DCL语句

DCL(Data Control Language)语句：数据控制语句，用于控制不同数据段直接的许可和访问级别的语句。这些语句定义了数据库、表、列、用户的访问权限和安全级别。

```sql
-- 查看用户权限
show grants for 用户名

```

## 8.1 授予数据库权限

授予数据库权限时，priv_type可以指定为以下值：

- SELECT：表示授予用户可以使用 SELECT 语句访问特定数据库中所有表和视图的权限。
- INSERT：表示授予用户可以使用 INSERT 语句向特定数据库中所有表添加数据行的权限。
- DELETE：表示授予用户可以使用 DELETE 语句删除特定数据库中所有表的数据行的权限。
- UPDATE：表示授予用户可以使用 UPDATE 语句更新特定数据库中所有数据表的值的权限。
- REFERENCES：表示授予用户可以创建指向特定的数据库中的表外键的权限。
- CREATE：表示授权用户可以使用 CREATE TABLE 语句在特定数据库中创建新表的权限。
- ALTER：表示授予用户可以使用 ALTER TABLE 语句修改特定数据库中所有数据表的权限。
- SHOW VIEW：表示授予用户可以查看特定数据库中已有视图的视图定义的权限。
- CREATE ROUTINE：表示授予用户可以为特定的数据库创建存储过程和存储函数的权限。
- ALTER ROUTINE：表示授予用户可以更新和删除数据库中已有的存储过程和存储函数的权限。
- INDEX：表示授予用户可以在特定数据库中的所有数据表上定义和删除索引的权限。
- DROP：表示授予用户可以删除特定数据库中所有表和视图的权限。
- CREATE TEMPORARY TABLES：表示授予用户可以在特定数据库中创建临时表的权限。
- CREATE VIEW：表示授予用户可以在特定数据库中创建新的视图的权限。
- EXECUTE ROUTINE：表示授予用户可以调用特定数据库的存储过程和存储函数的权限。
- LOCK TABLES：表示授予用户可以锁定特定数据库的已有数据表的权限。
- SHOW DATABASES：表示授权可以使用SHOW DATABASES语句查看所有已有的数据库的定义的权限。
- ALL或ALL PRIVILEGES：表示以上所有权限。

## 8.2 授予表权限

授予表权限时，priv_type可以指定为以下值：

- SELECT：授予用户可以使用 SELECT 语句进行访问特定表的权限。
- INSERT：授予用户可以使用 INSERT 语句向一个特定表中添加数据行的权限。
- DELETE：授予用户可以使用 DELETE 语句从一个特定表中删除数据行的权限。
- DROP：授予用户可以删除数据表的权限。
- UPDATE：授予用户可以使用 UPDATE 语句更新特定数据表的权限。
- ALTER：授予用户可以使用 ALTER TABLE 语句修改数据表的权限。
- REFERENCES：授予用户可以创建一个外键来参照特定数据表的权限。
- CREATE：授予用户可以使用特定的名字创建一个数据表的权限。
- INDEX：授予用户可以在表上定义索引的权限。
- ALL或ALL PRIVILEGES：所有的权限名。

## 8.3 授予列权限

授予列权限时，`priv_type`的值只能指定为SELECT、INSERT和UPDATE，同时权限的后面需要加上列名列表。

## 8.4 授予创建或删除用户权限

授予创建或删除权限时，`priv_type`的值指定为CREATE USER权限，具备创建用户、删除用户、重命名用户和撤消所有特权，而且是全局的。

## 8.5 授权语法格式

```sql
-- 有on，是授予权限，无on，是授予角色
-- 在on关键字后给出要授予权限的object_type
great 
	priv_type,[priv_type]... 
	on priv_level 
	to user [auth_option][,user [auth_option]]...

great '角色1','角色2' to 'user'@'localhost'
```

### priv_level的几种写法

- *：表示当前数据库中的所有表。
- .：表示所有数据库中的所有表。
- db_name.*：表示某个数据库中的所有表，db_name指定数据库名。
- db_name.tbl_name：表示某个数据库中的某个表或视图，db_name指定数据库名，tbl_name指定表名或视图名。
- tbl_name：表示某个表或视图，tbl_name指定表名或视图名。
- db_name.routine_name：表示某个数据库中的某个存储过程或函数，routine_name指定存储过程名或函数名。

### user的写法

- 使用%模糊匹配，符合匹配条件的主机可以访问该数据库实例，例如192.168.2.%或%.test.com；
- 使用localhost、127.0.0.1、::1及服务器名等，只能在本机访问；
- 使用ip地址或地址段形式，仅允许该ip或ip地址段的主机访问该数据库实例，例如192.168.2.1或192.168.2.0/24或192.168.2.0/255.255.255.0；
- 省略即默认为%

### auth_option的写法

auth_option为可选字段，可以指定密码以及认证插件(mysql_native_password、sha256_password、caching_sha2_password)。

## 8.6 移除权限

```sql
revoke 
	priv_type,[priv_type]... 
	on priv_level 
	from user [,user]...
```

# 9. TCL语句

TCL（Transaction Control Language）语句：事务控制语句

## 9.1 事务的特性

- 原子性：事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生
- 一致性：事务必须使数据库从一个一致性状态变换到另外一个一致性状态
- 隔离性：一个事务的执行不能被其他事务干扰
- 持久性：一个事务一旦被提交，它对数据库中数据的改变就是永久性的

```sql
-- 启动事务
set autocommit = 0;
-- 提交事务
commit;
-- 回滚事务
rollback;
-- 查看事务的隔离级别
select @@tx_isolation;
-- 设置当前连接的隔离级别
set session transaction isolation level read uncommitted;
-- 设置数据库全局的隔离级别
set global transaction isolation level read uncommitted;
```

# 10. 常用函数

## 10.1 数学函数

ABS(number)： 返回 number 的绝对值 

PI()： 计算圆周率及其倍数 

SQRT(x)： 返回非负数的x的二次方根

MOD(x,y)：返回x被y除后的余数

CEIL(x)：返回不小于x的最小整数

FLOOR(x)：返回不大于x的最大整数

ROUND(x)：对x进行四舍五入

ROUND(x,y)：其值保留到小数点后面y位；若y为负值，则将保留到x到小数点左边y位

POW(x,y)：返回x的y次乘方的值

EXP(x)：返回e的x乘方后的值

LOG(x)：返回x的自然对数，x相对于基数e的对数

LOG10(x)：返回x的基数为10的对数

SIN(x)：返回x的正弦

ASIN(x)：返回x的反正弦值

COS(x)：返回x的余弦

ACOS(x)：返回x的反余弦值

TAN(x)：返回x的正切

ATAN(x)：返回x的反正切值

COT(x)：返回x的余切

## 10.2 字符串函数

CHAR_LENGTH(str)：字符串的长度

CONCAT(s1, s2, …)： 一个或多个待拼接的内容，任意一个为NULL则返回值为NULL 

CONCAT_WS(x, s1, s2, …)： 返回多个字符串拼接之后的字符串，每个字符串之间有一个x 

INSERT(s1, x, len, s2)：返回字符串s1，其子字符串起始于位置x，被字符串s2取代len个字符

LOWER(str)：字母全部转换成小写

UPPER(str)：字母全部转换成大写

LEFT(s,n)：返回字符串s从最左边开始的n个字符

RIGHT(s,n)：后者返回字符串s从最右边开始的n个字符

LPAD(s1,len,s2)：返回s1，其左边由字符串s2填补到len字符长度，假如s1的长度大于len，则返回值被缩短至len字符

RPAD(s1,len,s2)：返回s1，其右边由字符串s2填补到len字符长度，假如s1的长度大于len，则返回值被缩短至len字符

LTRIM(s)：返回字符串s，其左边所有空格被删除

RTRIM(s)：返回字符串s，其右边所有空格被删除

TRIM(s)：返回字符串s，删除两边空格之后的字符串

TRIM(s1 FROM s)：删除字符串s两端所有子字符串s1，未指定s1的情况下则默认删除空格

- 作用：删除字符串s两端所有子字符串s1，未指定s1的情况下则默认删除空格

REPEAT(s, n)：返回一个由重复字符串s组成的字符串，字符串s的数目等于n

SPACE(n)：返回一个由n个空格组成的字符串

REPLACE(s, s1, s2)：返回一个字符串，用字符串s2替代字符串s中所有的字符串s1

- 作用：返回一个字符串，用字符串s2替代字符串s中所有的字符串s1

STRCMP(s1, s2)：若s1和s2中所有的字符串都相同，则返回0；根据当前分类次序，小于则返回-1，大于则返回1

SUBSTRING(s, n, len)：从字符串s中返回一个第n个字符开始、长度为len的字符串

LOCATE(str1, str)：返回子字符串str1在字符串str中的开始位置

REVERSE(s)：将字符串s反转

ELT(N, str1, str2, str3, str4, …)：返回第N个字符串

## 10.3 日期和时间函数

CURDATE()

CURRENT_DATE()：返回当前的日期，将当前日期按照"YYYY-MM-DD"或者"YYYYMMDD"格式的值返回

CURRENT_TIMESTAMP()：返回当前日期和时间，格式为"YYYY_MM-DD HH:MM:SS"或"YYYYMMDDHHMMSS"

UNIX_TIMESTAMP()：返回一个格林尼治标准时间1970-01-01 00:00:00到现在的秒数

UNIX_TIMESTAMP(date)：返回一个格林尼治标准时间1970-01-01 00:00:00到指定时间的秒数

FROM_UNIXTIME(date)
作用：和UNIX_TIMESTAMP互为反函数，把UNIX时间戳转换为普通格式的时间

UTC_DATE()：返回当前UTC（世界标准时间）日期值，其格式为"YYYY-MM-DD"或"YYYYMMDD"

UTC_TIME()：返回当前UTC时间值

MONTH(date)：返回指定日期中的月份

MONTHNAME(date)：返回指定日期中的月份的名称

DAYNAME(date)：返回指定日期对应的星期几

DAYOFWEEK(date)：返回指定日期对应的星期几的索引，1表示周日，2表示周一...

WEEKDAY(date)：返回指定日期对应的星期几的索引，0表示周一，1表示周二...

WEEK(date)：返回指定日期是一年中的第几周

DAYOFYEAR(date)：返回指定日期是一年中的第几天

DAYOFMONTH(date)：返回指定日期是该月中的第几天

YEAR(date)：返回指定日期对应的年份，范围是1970~2069

QUARTER(date)：返回指定日期对应一年中的季度，范围是1~4

MINUTE(time)：返回指定时间对应的分钟数，范围是0~59

SECOND(time)：返回指定时间对应的秒数

EXTRACE(type FROM date)：从日期中提取一部分，type可以是YEAR、YEAR_MONTH、DAY_HOUR、DAY_MICROSECOND、DAY_MINUTE、DAY_SECOND

TIME_TO_SEC(time)：返回指定时间转换成的秒数

SEC_TO_TIME()：和TIME_TO_SEC(time)相反，将秒数转换为时间格式

DATE_ADD(date, INTERVAL expr type)：返回将起始时间加上expr type之后的时间

DATE_SUB(date, INTERVAL expr type)：返回将起始时间减去expr type之后的时间

## 10.4 条件判断函数

IF(expr, v1, v2)：如果expr是TRUE，则返回v1；否则返回v2

IFNULL(v1, v2)：如果v1不为NULL，则返回v1；否则返回v2

CASE expr WHEN v1 THEN r1 [WHEN v2 THEN v2] [ELSE rn] END：如果expr等于某个vn，则返回对应位置THEN后面的结果，如果与所有值都不想等，则返回ELSE后面的rn

## 10.5 系统函数

VERSION()：查看MySQL版本号

CONNECTION_ID()：查看当前用户的连接数

USER()：查看当前被MySQL服务器验证的用户名和主机

CHARSET(str)：查看字符串使用的字符集

COLLATION(str)：查看字符串排列方式

## 10.6 其他函数

FORMAT(x, n)：将数字x格式化，并以四舍五入的方式保留小数点后n位，结果以字符串形式返回

CONV(N, from_base,  to_base)：不同进制数之间的转换，返回N，由from_base进制转换为to_base进制

CONVERT(str USING charset)：使用字符集charset表示字符串str


- 作用：查看字符串str使用的字符集

- 作用：返回第N个字符串

- 作用：将字符串s反转

- 作用：若s1和s2中所有的字符串都相同，则返回0；根据当前分类次序，第一个参数小于第二个则返回-1，其他情况返回1

- 作用：返回一个由n个空格组成的字符串

- 作用：返回一个由重复字符串s组成的字符串，字符串s的数目等于n

- 作用：返回字符串s删除了两边空格之后的字符串

- 作用：前两者将str中的字母全部转换成小写，后两者将字符串中的字母全部转换成大写

- 作用：返回字符串s1，其子字符串起始于位置x，被字符串s2取代len个字符