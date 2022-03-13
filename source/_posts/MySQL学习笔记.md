---
title: MySQL学习笔记
categories:
	- MySQL
tags:
	- Java
	- MySQL
	- 数据库
description:
	MySQL数据库入门
keywords: MySQL
date: 2019-09-14
---

这篇文章是记录我学习 `MySQL` 时的一些笔记，不成体系，仅为以后快速学习准备，由于记录的是从零学习 `MySQL` 的笔记，所以笔记的内容是十分的基础的，基本上是以用为主，不牵涉到具体的原理性问题。

## MySQL基本概念

我们使用Java编写程序，一般数据都是存储在内存中的，一旦程序终止或断电，那么数据就会丢失，所以我们需要将数据存储到本地文件中，我们一般存储到数据库中，而 `MySQL` 正是这么一款数据库。

安装 `MySQL` 很简单，直接使用搜索引擎搜索 `MySQL`，进入官网进行安装，`MySQL` 是开源的软件。目前企业使用的是5.5-5.7的版本，选择进行下载即可。若要卸载 `MySQL`，在手动卸载 `MySQL` 后，还要删除 `C:\ProgramData\MySQL` 这个文件夹，否则重新安装时不能成功。

### 连接MySQL

在命令行中输入

```bash
mysql -uroot -p
```

然后会提示你输入密码

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203163117.png" width="40%"/>

输入的密码以密文的形式显示，以保证安全。当然你也可以直接在 `-p` 后面输入密码，不过这样并不安全。

### SQL语句分类

`SQL` 语句按功能分为

- `DDL`：操作数据库，表
- `DML`：增、删、改数据
- `DQL`：查询数据
- `DLL`：与授权有关

在 `MySQL` 中不区分大小写，不过关键字一般会大写，而数据库名和表名一般小写，`SQL`语句需要以`;`结尾，否则会一直等待输入。

### 注释

`MySQL` 的注释为 `--` 后面需要接一个空格，否则会报错，另一种注释为 `#`，这是 `MySQL` 独有的，后面不需要加空格

```sql
show databases; -- 显示所有的数据库
show databases; #显示所有的数据库
```

## DDL

这里来介绍操作数据库和表的 `SQL` 语句，这些操作一般就是 `C(Create), R(Retrieve), U(Update), D(Delete)`。

### 操作数据库

#### 查询

首先介绍查询数据库的语句，连接上 `MySQL` 后，在命令行中输入(这里关键字没有大写)

```sql
show databases;
```

这个语句的作用是显示出所有的数据库

```mysql
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| world              |
+--------------------+
6 rows in set (0.31 sec)
```

下面这条语句的作用是显示出某数据库所用的字符集

```sql
show create database 数据库名;
```

比如

```sql
show create database world;
```

输出为

```sql
+----------+------------------------------------------------------------------+
| Database | Create Database                                                  |
+----------+------------------------------------------------------------------+
| world    | CREATE DATABASE `world` /*!40100 DEFAULT CHARACTER SET latin1 */ |
+----------+------------------------------------------------------------------+
1 row in set (0.05 sec)
```

可见 `world` 数据库所用的字符集为 `latin1`。

#### 创建

创建数据库的语句为

```sql
create database 数据库名;
```

如

```sql
create database db1; -- 创建一个名为db1的数据库
```

如果数据库已经存在，那么会发生错误，例如再次执行上面的命令 `create database db1;` 会发生如下错误

```sql
ERROR 1007 (HY000): Can't create database 'db1'; database exists
```

这个时候我们可以使用

```sql
create database if not exists db1; -- 如果db1不存在，那么创建db1，否则什么也不做
```

我们还可以在创建数据库时指定字符集，如

```sql
create database db2 character set gbk;
```

使用 `show create database db2;` 查看字符集

```sql
+----------+-------------------------------------------------------------+
| Database | Create Database                                             |
+----------+-------------------------------------------------------------+
| db2      | CREATE DATABASE `db2` /*!40100 DEFAULT CHARACTER SET gbk */ |
+----------+-------------------------------------------------------------+
1 row in set (0.03 sec)
```

#### 修改

我们可以通过下面的命令修改数据库的字符集

```sql
alter database 数据库名 character set 字符集;
```

例如修改 `db2` 数据库为 `utf8` 编码

```sql
alter database db2 character set utf8;
```

#### 删除

我们可以使用 `drop` 命令删除数据库，例如

```sql
drop database db2;
```

使用 `show databases;` 查看数据库，发现 `db2` 已经被删除了

```sql
+--------------------+
| Database           |
+--------------------+
| information_schema |
| db1                |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| world              |
+--------------------+
```

如果数据库不存在，那么会报错，比如在删除一次 `db2`

```sql
ERROR 1008 (HY000): Can't drop database 'db2'; database doesn't exist
```

这个时候我们可以使用下面的语句

```sql
drop database if exists db2; -- 如果db2存在则删除db2，否则什么也不做
```

#### 使用数据库

通过 `select database();` 命令可以查看我们正在使用哪一个数据库

```sql
+------------+
| database() |
+------------+
| NULL       |
+------------+
1 row in set (0.00 sec)
```

因为我们没有使用数据库，所以这里显示的是 `NULL`，可以通过 `use 数据库名;`来使用数据库，比如使用 `db1` 数据库

```sql
use db1;
```

再次执行 `select database();` 输出为

```sql
+------------+
| database() |
+------------+
| db1        |
+------------+
1 row in set (0.00 sec)
```

### 操作表

#### 查询

可以使用 `show tables` 查询某数据库中所有的表，例如现在我们使用 `world` 数据库，然后查询其中所有的表

```sql
use world;
show tables;
```

结果为

```sql
+-----------------+
| Tables_in_world |
+-----------------+
| city            |
| country         |
| countrylanguage |
+-----------------+
3 rows in set (0.09 sec)
```

我们还可以使用 `desc 表名;` 来查询某表的结构，我们来查询 `city` 表的结构

```sql
desc city;
```

```sql
+-------------+----------+------+-----+---------+----------------+
| Field       | Type     | Null | Key | Default | Extra          |
+-------------+----------+------+-----+---------+----------------+
| ID          | int(11)  | NO   | PRI | NULL    | auto_increment |
| Name        | char(35) | NO   |     |         |                |
| CountryCode | char(3)  | NO   | MUL |         |                |
| District    | char(20) | NO   |     |         |                |
| Population  | int(11)  | NO   |     | 0       |                |
+-------------+----------+------+-----+---------+----------------+
5 rows in set (0.65 sec)
```

使用 `show create table 表名;` 查看表的字符集。

#### 创建

创建表的语法为

```sql
create table 表名(列名1 数据类型1, ..., 列名n 数据类型n);
```

MySQL 中常用的数据类型有

- `int`
  - 整数类型
- `double`
  - 浮点数类型，接收两个参数，如 `double(5,2)`，5 代表数字的总长度，2 代表小数点后的位数
- `date`
  - 日期类型，格式为 `yy-MM-dd`
- `datetime`
  - 日期类型，格式为`yy-MM-dd HH:mm:ss`
- `timestamp`
  - 时间戳，格式为`yy-MM-dd HH:mm:ss`，当不赋值或赋值为NULL时，自动使用当前的时间作为值
- `varchar`
  - 字符串类型，接收一个参数表示字符串的最大长度，如 `varchar(20)`

现在我们在 db1 中创建一个 student 表

```sql
use db1; -- 使用数据库db1
create table student(name varchar(10), age int, score double(4,1), insert_time timestamp); -- 创建表student 里面包括name age score insert_time 等列
desc student; -- 查看student表的结构
```

```sql
+-------------+-------------+------+-----+-------------------+-----------------------------+
| Field       | Type        | Null | Key | Default           | Extra                       |
+-------------+-------------+------+-----+-------------------+-----------------------------+
| name        | varchar(10) | YES  |     | NULL              |                             |
| age         | int(11)     | YES  |     | NULL              |                             |
| score       | double(4,1) | YES  |     | NULL              |                             |
| insert_time | timestamp   | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
+-------------+-------------+------+-----+-------------------+-----------------------------+
4 rows in set (0.07 sec)
```

#### 删除

同删除数据库一样，有两种用法

```sql
drop table 表名;
drop table if exists 表名;
```

这里不多做解释。

#### 修改

- 重命名表名

```sql
alter table 表名 rename to 新表名;
```

- 修改表的字符集

```sql
alter table 表名 character set 字符集;
```

- 添加一列

```sql
alter table 表名 add 列名 数据类型;
```

- 修改列名及其类型

```sql
alter table 表名 change 列名 新列名 新数据类型;
```

- 修改列的数据类型

```sql
alter table 表名 modify 列名 新数据类型;
```

- 删除列

```sql
alter table 表名 drop 列名;
```

## DML

`DML` 是与修改数据有关的 `sql` 语句。修改数据主要包括的是增删改数据。为了查看修改数据的效果，这里介绍一个查询数据的命令

```sql
select * from 表名;
```

现在创建一个 `student` 的表

```sql
CREATE TABLE student (
    name varchar(10), -- 名字长度最大10个字符
    age int, -- 年龄
    math_score double(3,1), -- 数学成绩
    english_score double(3,1), -- 英语成绩
    insert_time timestamp  -- 数据加入的时间
); 
```

```sql
DESC student; -- 查看表的结构
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203163637.png"/>

### 添加数据

向表中添加数据的写法为

```sql
insert into 表名(列名1, 列名2, ..., 列名n) values(值1, 值2, ..., 值n); -- 列名和值要一一对应
```

例如向表中添加数据

```sql
INSERT INTO student (name, age, math_score, english_score) VALUES ('dilireba', 27, 60, 70);
INSERT INTO student (name, age, math_score, english_score) VALUES ('gulinazha', 28, 62, 68);
```

使用 `SELECT * FROM student;` 得到数据为

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203163828.png"/>

注意：

- 列名和值名要一一对应
- 如果表名后不定义列名，则默认为给所有列添加，如
  - `INSERT INTO student VALUES ('dilireba', 27, 60, 70);`
- 除了数字类型，其他类型要用引号(`'`)括起来

### 删除数据

删除数据的格式为

```sql
delete from 表名 [where 条件]; -- 删除满足条件的行
```

其中 `[]` 代表的是里面的内容可省略，如果不加条件的话，默认为删除表中的所有数据，现在我们删除上例中 `age > 27` 的行，如下

```sql
DELETE FROM student WHERE age > 27;
```

得到的结果为

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203164212.png"/>

如果要删除表中的所有数据的话，不推荐使用

```sql
delete from 表名;
```

因为它会将表中的数据一行一行的删除，效率较慢，推荐使用

```sql
truncate table 表名;
```

它会直接删除这个表，然后创建一个空表，这个表的名字和结构与删除的表相同，从效果上就相当于是删除了表中所有的数据，但是它的效率比 `delete from 表名;` 快。

### 更新数据

更新数据的语法为

```sql
update 表名 set 列名1 = 值1, ..., 列名n = 值n [where 条件]; -- 当符合条件时，更新值
```

如何省略条件，那么会修改所有的行，如现在我要更新，如何符合条件 `age = 27`，那么将 `age` 修改为 `28`，如下

```sql
UPDATE student set age = 28 WHERE age = 27;
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203164242.png"/>

## DQL

查询数据的基本语法为

```sql
SELECT 
    字段列表
FROM 
    表名列表
WHERE 
    条件列表
GROUP BY 
    分组字段
HAVING 
    分组后的条件限定
ORDER BY 
    排序
LIMIT 
    分页限定
```

下面会详细的讲解其中各个关键字的意思。创建一个 `student` 表如下

```sql
CREATE TABLE student (
    id int,  --  编号
    name varchar(20), --  姓名
    age int, --  年龄
    sex varchar(5),  --  性别
    address varchar(100),  --  地址
    math int, --  数学
    english int --  英语
);
```

向其中插入以下数据

```sql
INSERT INTO student(id,name,age,sex,address,math,english) VALUES 
       (1,'马云',55,'男', '杭州 ',66,78),
       (2,'马化腾 ',45,'女 ','深圳 ',98,87),
       (3,'马景涛 ',55,'男 ','香港 ',56,77),
       (4,'柳岩',20,'女 ','湖南 ',76,65),
       (5,'柳青 ',20,'男 ','湖南 ',86,NULL),
       (6,'刘德华 ',57,'男 ','香港',99, 99),
       (7,'马德',22,'女','香港',99,99),
       (8,'德玛西亚',18,'男','南京',56,65);
```

如果提示

```sql
Incorrect string value: '\xE9\xA9\xAC\xE4\xBA\x91' for column 'name' at row 1
```

那么就是因为字符编码的问题，这时可以修改 `name,address,sex` 的字符集为 `utf8`

```sql
alter table student change name name char(20) character set utf8;
alter table student change address address char(100) character set utf8;
alter table student change sex sex char(5) character set utf8;
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203164316.png"/>

### 基础查询

#### 查询多个字段

```sql
SELECT 
	name, age 
FROM 
	student; -- 查询name和age字段
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203164546.png" width="30%"/>

#### 去重

查询 `address` 字段时，发现有相同的，如"香港"

```sql
SELECT 
	address 
FROM 
	student;
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203164700.png" width="25%"/>

如果希望重复出现的只出现一次，那么可以使用 `DISTINCT`

```sql
SELECT DISTINCT 
	address 
FROM 
	student;
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203164805.png" width="25%"/>

#### 计算列

现在要计算它们的数学成绩和英语成绩的和

```sql
SELECT 
	name, math, english, math+english 
FROM 
	student; -- 为了看出是谁的总分，这里加上一个name字段
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203164847.png" width="70%"/>

我们发现第 5 行的结果为 `null`，这是因为 `null`+ 其它数得到的结果都是 `null`，但是这里我们应该把 `null` 当做 0 处理 ，这样加出来的就是总分，而不是 `null`

```sql
SELECT 
	name, math, english, math+ifnull(english,0) 
FROM 
	student;
```

这里使用了 `ifnull(english,0)`，如果 `english` 的值是 `null`，那么就替换为 0，所以我们得到的结果为

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203165223.png" width="80%"/>

#### 起别名

我们注意到上面的最后一列的列名为 `math+ifnull(english,0)`，这个列名没有什么意义，我们应该为它起个别名，如 `score`

```sql
SELECT 
	name, math, english, math+ifnull(english,0) AS score 
FROM 
	student;
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203165334.png" width="60%"/>

其中 `AS` 作为一个起别名的作用，`AS` 其实可以省略，使用空格替代即可，如

```sql
SELECT 
	name, math, english, math+ifnull(english,0) score 
FROM 
	student;
```

该句得到的结果与上面的相同

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203165334.png" width="60%"/>

### 条件查询

我们使用 `WHERE` 来指明条件查询，比如我要查询年龄在 20 岁以上的

```sql
SELECT 
	name,age 
FROM 
	student 
WHERE 
	age > 20;
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203165503.png" width="30%"/>

可见 `WHERE` 后面跟的是一个逻辑值，既然是逻辑值就可以使用与或非运算

- `AND`
- `OR`
- `NOT`

```sql
SELECT 
	name,age 
FROM 
	student 
WHERE 
	age > 20 AND age < 50; -- 年龄在20-50之间的
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203165700.png" width="30%" />

我们可以使用 `BETWEEN ... AND ...` 来简化上面的操作

```sql
SELECT 
	name,age 
FROM 
	student 
WHERE 
	age BETWEEN 20 AND 50; -- 在20-50之间，包括20和50
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203165802.png" width="30%"/>

```sql
SELECT 
	name,age 
FROM 
	student 
WHERE 
	age = 18 OR age = 20 OR age = 25; -- 查询年龄为18或20或25的
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203170207.png" width="30%"/>

我们可以使用 `IN` 达到相同的效果

```sql
SELECT 
	name,age 
FROM 
	student 
WHERE 
	age IN (18,20,25); -- 查询年龄为18或20或25的

```

### 排序查询

使用 `ORDER BY` 来对查询结果进行排序，后面跟要排序的字段，默认对字段进行升序排序。

- `ASC`：升序
- `DESC`：降序

```sql
SELECT 
	name,math 
FROM 
	student 
ORDER BY 
	math; -- 默认为升序

```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203170258.png" width="30%"/>

```sql
SELECT 
	name,math 
FROM 
	student 
ORDER BY 
	math DESC; -- 降序
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203170333.png" width="30%"/>

`ORDER BY` 可以对多个字段进行排序，先对前面的字段进行排序，如果前面的字段相同，在根据后面的字段排序，比如按照数学和英语成绩排名，优先按数学成绩来，如果数学成绩相同则按英语成绩来

```sql
SELECT 
	name,math,english 
FROM 
	student 
ORDER BY 
	math DESC, english DESC;
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203170405.png" width="50%"/>

### 模糊查询

使用 `LIKE` 进行模糊查询，比如记不住全称，这时可以使用模糊查询，比如想查询姓马的，在查询之前要介绍占位符

- `_`：表示单个任意字符
- `%`：表示多个任意字符

```sql
SELECT 
	name 
FROM 
	student 
WHERE 
	name LIKE '马%';
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201203170436.png" width="15%"/>

如果我想查询名字中带有"德"字的

```sql
SELECT 
	name 
FROM 
	student 
WHERE 
	name LIKE '%德%';
```

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203171638.png" width="15%"/>

### 聚合函数

聚合函数将一列作为整体，进行纵向计算。聚合函数有

- `count`：统计个数，不包括 `NULL`。
- `max`：计算最大值
- `min`：计算最小值
- `sum`：计算总和
- `avg`：计算平均值

```sql
SELECT count(math) FROM student;
SELECT max(math) FROM student;
SELECT min(math) FROM student;
SELECT sum(math) FROM student;
SELECT avg(math) FROM student;
```

### 分组查询

使用 `GROUP BY` 进行分组查询，比如我想男生和女生的数学平均分，那么可以使用

```sql
SELECT 
     sex,avg(math) avg  -- avg是别名
FROM 
     student 
GROUP BY 
     sex;
```

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203171733.png" width="25%"/>

如果我想对分数在 70 以上的人计算平均分，并且统计人数，可以这么写

```sql
SELECT 
     sex,avg(math) avg, count(id) count -- avg和count是别名
FROM 
     student 
WHERE 
     math > 70 
GROUP BY 
     sex;
```

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203172109.png" width="40%"/>

如果分组后的还要进行筛选，那么可以使用 `HAVING`，比如这里我要筛选分组后人数大于 2 的才进行统计

```sql
SELECT
       sex,avg(math) avg, count(id) count
FROM
     student
WHERE
      math > 70
GROUP BY
         sex
HAVING
    count(id) > 2;
```

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203172158.png" width="40%"/>


### 分页查询

当我们查询数据时，如果我们一页只能显示几行数据，我们就要进行分页查询，使用 `LIMIT`，后面跟两个数，第一个数代表查询的起始位置，从 0 开始，第二个代表一页显示的行数，如

```sql
SELECT
       *
FROM
    student
LIMIT
    0,3; -- 查询中0开始的3行数据
```

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203172238.png"/>

如果我们要查询第二页，可以怎么写

```sql
SELECT
       *
FROM
    student
LIMIT
    3,3;
```

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203173134.png"/>


## 约束

所谓约束就是对数据产生限制，比如说某列不能为空(NULL)，有或者说某列的数据不能重复。约束一般包括下面四种约束

- 非空约束(not null)
- 唯一约束(unique)
- 主键约束(primary key)
- 外键约束(foreign key)

### 非空约束

所谓的非空约束指的就是该列不能有 `NULL` 值，下面介绍如何创建非空约束，分为两种情况，一种是在创建表示添加非空约束，一种是在创建表之后添加非空约束

1. 创建表示添加非空约束

```sql
CREATE TABLE emp(
    id int,
    name varchar(10) not null); -- 为name添加非空约束 name不能为NULL
```

如果此时为 name 赋值为 `NULL`，那么这条语句将会报错

```sql
INSERT INTO emp(id,name) VALUES (0,NULL); -- 为name赋值为NULL 将会报错
```

```sql
Column 'name' cannot be null
```

2. 在创建表后添加非空约束

如现在给 id 也添加非空约束，应当这么写

```sql
ALTER TABLE emp MODIFY id int not null; -- 为id添加非空约束
```

3. 删除非空约束

删除的办法与添加的语法差不多，如现在我又要删除 id 的非空约束，应当这么写

```sql
ALTER TABLE emp MODIFY id int; -- 删除id的非空约束
```

### 唯一约束

唯一约束指的是该列的值不能相同，关键字为 `unique`。

1. 创建表时如何添加唯一约束

```sql
CREATE TABLE emp(
    id int UNIQUE,
    name varchar(10)); -- 为name添加非空约束 name不能为NULL
```

2. 创建表后添加唯一约束

```sql
ALTER TABLE emp MODIFY id int UNIQUE;
```

3. 删除唯一约束

删除唯一约束的语法与删除非空约束的语法不同，如下

```sql
ALTER TABLE emp DROP INDEX id;
```

### 主键约束

主键约束`(primary key)`是上面两个的总和，即该列既不能为 `NULL`，也不能相同。一张表只能有一个字段为主键。

1. 创建表时添加主键

```sql
CREATE TABLE emp(
    id int PRIMARY KEY,
    name varchar(10)); 
```

2. 创建表后添加主键

```sql
ALTER TABLE emp MODIFY id int PRIMARY KEY;
```

3. 删除主键

```sql
ALTER TABLE emp DROP PRIMARY KEY ; -- 因为主键只有一个，不必指明是哪个字段
```

下面介绍一个小知识点，主键自动增长，当我们添加数据时，如果我们设置了主键增长并且没有为主键赋值，那么主键的值会相较于上一条数据主键的值增长，设置主键自动增长的语法为(假设设置(了) id 为主键)

```sql
CREATE TABLE emp(
    id int PRIMARY KEY AUTO_INCREMENT, -- 创建表时添加
    name varchar(10) ); 

ALTER TABLE emp MODIFY id int AUTO_INCREMENT; -- 创建表后添加 id已经设置为主键了
```

删除主键增长的方法为

```sql
ALTER TABLE emp MODIFY id int; -- 这样是不会删除主键的，只会删除主键自动增长
```

### 外键约束

假设有这么一张表

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203173213.png"/>

观察研发部门和部门地点，发现数据冗余很严重，并且在后续添加数据中添加的也是这么一对一对的，很麻烦并且有在添加数据可能会出错，所以我们可以把这张表拆成两张表，如下

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203173238.png"/>

现在问题是怎么将这两张表联系起来，答案就是外键约束。那么什么是外键，从表(被别人约束的表)中与主表(用来约束别人的表)主键对应的那一列，如：员工表中的 dep_id。

1. 新建表时增加外键

```sql
[CONSTRAINT] [外键约束名称] FOREIGN KEY(外键字段名) REFERENCES 主表名(主键字段名)
```

2. 创建表后添加外键

```sql
ALTER TABLE 从表 ADD [CONSTRAINT] [外键约束名称] FOREIGN KEY (外键字段名) REFERENCES  主表(主
键字段名);
```

比如我在创建 employee 表时添加外键

```sql
CREATE TABLE employee(
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20),
    age INT,
    dep_id INT,  --  外键对应主表的主键
    CONSTRAINT emp_depid_fk FOREIGN KEY (dep_id) REFERENCES department(id)
);
```

employee 表已经存在的情况下添加外键

```sql
ALTER TABLE employee ADD CONSTRAINT emp_depid_fk FOREIGN KEY (dep_id) REFERENCES department(id);

```

3. 删除外键

```sql
--  删除 employee 表的 emp_depid_fk 外键
ALTER TABLE employee DROP FOREIGN KEY emp_depid_fk;

```

这个时候又会出现新的问题，比如我改变主表中 id=1 为 id=5，但是这时是不会成功的，因为如果改了，从表中的 dep_id 便没有对应的值，这个时候就需要级联操作(在修改和删除主表的主键时，同时更新或删除副表的外键值，称为级联操作)。

- `ON UPDATE CASCADE` 
  - 级联更新，只能是创建表的时候创建级联关系。更新主表中的主键，从表中的外键
    列也自动同步更新
- `ON DELETE CASCADE`
  - 级联删除

```sql
create table employee(
    id int primary key auto_increment,
    name varchar(20),
    age int,
    dep_id int,  --  外键对应主表的主键
    --  创建外键约束
    constraint emp_depid_fk foreign key (dep_id) references department(id) on update cascade on delete cascade
);
```

## 三大范式

设计数据库时，需要遵循的一些规范。要遵循后边的范式要求，必须先遵循前边的所有范式要求。设计关系数据库时，遵从不同的规范要求，设计出合理的关系型数据库，这些不同的规范要求被称为不同的范式，各种范式呈递次规范，越高的范式数据库冗余越小。

目前关系数据库有六种范式 ： 第一范式(1NF) 、第二范式(2NF) 、第三范式(3NF) 、巴斯-科德范式(BCNF) 、第四范式(4NF)和第五范式(5NF，又称完美范式) 。满足最低要求的范式是第一范式(1NF) 。在第一范式的基础上进一步满足更多规范要求的称为第二范式(2NF) ，其余范式以次类推。一般说来，数据库只需满足第三范式(3NF)就行了。

为了理解三大范式，我们首先来看这么一张表

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203220153.png" width="60%"/>

我们首先来看第一范式的概念：数据库表的每一列都是不可分割的原子数据项。即表中的某个列有多个值时，必须拆分为不同的列。上面的那个表不满足第一范式，因为系的那一列被拆成了两列，我们将它拆分成不同的列

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203220352.png" width="60%"/>

它现在满足第一范式的要求了，接下来看第二范式的概念：在1NF的基础上，非码属性必须完全依赖于码。为了理解这句话的意思，先看下面几个概念：

1. 函数依赖：`A=>B`，如果通过 A 属性(属性组)的值，可以确定唯一 B 属性的值。则称 B 依赖于 A 
   - 例如：学号`=>`姓名。(学号，课程名称) `-->` 分数
2. 完全函数依赖：`A=>B`， 如果 A 是一个属性组，则 B 属性值得确定需要依赖于 A 属性组中所有的属性值。
   - 例如：(学号，课程名称) `=>` 分数
3. 部分函数依赖：`A=>B`， 如果 A 是一个属性组，则 B 属性值的确定只需要依赖于 A 属性组中某一些值即可。
   - 例如：(学号，课程名称) `-->` 姓名，只依靠学号
4. 传递函数依赖：`A=>B`, `B=>C` . 如果通过 A 属性(属性组)的值，可以确定唯一 B 属性的值，在通过 B 属性(属性组)的值可以确定唯一 C 属性的值，则称 C 传递函数依赖于 A
   - 例如：学号 `=>` 系名，系名 `=>` 系主任
5. 码：如果在一张表中，一个属性或属性组，被其他所有属性所完全依赖，则称这个属性(属性组)为该表的码
   - 例如：该表中码为：(学号，课程名称)
   - 主属性：码属性组中的所有属性
   - 非主属性：除过码属性组的属性

其实 2NF 就是在 1NF 基础上消除非主属性对主码的部分函数依赖，上表中的码为(学号，课程名称)，只有分数完全依赖该码组，所以我们拆分表如下

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203220446.png" width="80%"/>

第三范式(3NF)：在2NF基础上，任何非主属性不依赖于其它非主属性(在2NF基础上消除传递依赖)，在学生表中存在学号 `=>` 系名 `=>` 系主任的传递依赖，所以再次拆分表

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203220738.png" width="70%" />

三大范式小结

- 1NF: 原子性，表中每列不可再拆分。
- 2NF: 不产生局部依赖，一张表只描述一件事情
- 3NF: 不产生传递依赖，表中每一列都直接依赖于主键。而不是通过其它列间接依赖于主键。 

## 多表查询

多表查询顾名思义就是同时查询多张表，假设有下面这么两张表(第一张是职员(emp)表，第二张是部门(dept)表)

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203220840.png"/>

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203221449.png" width="30%"/>

现在我们同时查询这两张表

```sql
SELECT
    *
FROM
    emp, dept;
```

我们将得到下面这么一张表

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203221704.png"/>

这张表的结果是两张表一一组合得到的，得到的结果也叫做笛卡尔积，我们可以看到很有的信息都是错误的，我们的目的就是去除这些无用的信息。

### 内连接

用左边表的记录去匹配右边表的记录，如果符合条件的则显示。内连接分为

- 隐式内连接
  - 使用 `WHERE` 条件指定
- 显示内连接
  - 使用 `INNER JOIN ... ON` 语句

比如现在我要在上面的笛卡尔积中筛选出 emp 表外键 dept_id 等于主表主键 id 的，那么分别使用隐式内连接和显式外连接的写法为

```sql
#隐式写法
SELECT
    *
FROM
    emp, dept
WHERE
    emp.dept_id = dept.id;
    
#显式写法
SELECT * FROM emp INNER JOIN dept ON emp.dept_id = dept.id;
```

### 外连接

为了演示外连接，在上面的部门表中新加入一个销售部

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203221735.png" width="30%"/>

外连接分为

- 左外连接
- 右外连接

两者只要掌握一种即可，因为用另一种时，将二表的顺序交换即可。那什么是左外连接，就是在内连接的基础上，以左表为基准，显示左表的所有内容，如果右表没有对应的内容，那么显示为 `NULL`，现在我们进行一次内连接查询

```sql
SELECT * FROM dept INNER JOIN emp e ON e.dept_id = dept.id; # e是emp的别名
```

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203221806.png"/>

现在我们进行一次左外连接查询

```sql
SELECT * FROM dept LEFT JOIN emp e ON dept.id = e.dept_id;
```

因为左外连接查询是以左表(dept)为基准，左表的内容会全部显示出来，即销售部会被查询出来，而对应的员工表没有对应的元素，所以会显示空

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203221833.png"/>

而右外连接与左外连接相反，是以右表为基准，现在如果我们将二表的位置交换，并且使用右外连接查询，得到的结果与上面的会是相同的

```sql
SELECT * FROM emp e RIGHT JOIN dept d ON e.dept_id = d.id;
```

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203222043.png"/>

### 子查询

所谓的子查询是指将查询得到的结果作为另一个查询语句的条件，比如我想查出薪资最高员工的信息，那么思路如下

- 查出最高的薪资是多少
- 匹配谁的薪资为最高薪资

那么第一步查出的最高薪资就作为了第二步进行匹配的条件

```sql
SELECT * FROM emp t1 WHERE t1.salary = (SELECT max(salary) FROM emp);
```

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203222108.png"/>

子查询得到的结果有多种，例如

- 单行单列
- 单列多行
- 多行多列的值(表)

当结果是单个列的值的时候，肯定在 `WHERE` 后面作为条件，父查询使用比较运算符，如：`> 、<、<>、 =` 等。现在我要查询小于平均薪资的人有哪些，那么可以这么写

```sql
SELECT * FROM emp t1 WHERE t1.salary < (SELECT avg(salary) FROM emp);
```

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203222137.png"/>

子查询结果是单列多行，结果集类似于一个数组，父查询使用 IN 运算符。查询工资大于5000的员工，来自于哪些部门

```sql
SELECT
    t1.name
FROM
     dept t1
WHERE
    t1.id IN (SELECT emp.dept_id FROM emp WHERE emp.salary > 5000);
```

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203222157.png" width="18%"/>

子查询结果只要是多列，肯定在FROM后面作为表

```sql
SELECT 查询字段 FROM （子查询）表别名 WHERE 条件;
```

子查询作为表需要取别名，否则这张表没有名称则无法访问表中的字段。查询出2011年以后入职的员工信息，包括部门名称 

```sql
SELECT
    *
FROM
    (SELECT * FROM db1.emp t1 WHERE t1.join_date > "2011-1-1") t3, dept d
WHERE
    d.id = t3.dept_id;
```

## 事务

所谓事务是指一系列的操作，这些操作要么同时成功，要么同时失败。比如转账，不能我这里转账成功，你那么收不到钱，那么钱就这么消失了? 如果事务执行成功了，那么就提交，如果有一条失败了，那么就需要进行回滚(即回到之前的状态)。与事务有关的三条语句为

- `start transaction`
  - 开启事务
- `commit`
  - 提交事务
  - 可手动提交，也可设置为自动提交
- `roll back`
  - 回滚事务
  - 当事务执行失败时自动自动，也可手动执行

假设有这么一张表，里面存储的是用户名和余额信息

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting/20201203222234.png" width="50%"/>

现在张三要向李四转账 500 块，如下

```sql
-- 张三账号-500 
update account set balance = balance - 500 where name='张三'; 
-- 李四账号+500 
update account set balance = balance + 500 where name='李四'; 
```

如果我们不开启事务的话，那么当张三转了 500 块时，这时服务器崩溃了，李四没有收到钱，但是钱还是少了，这种情况是不能发生的，我们应当开启一个事务，这两个操作要么同时成功，要么同时失败。

```sql
START TRANSACTION; -- 开启一个事务

-- 张三账号-500
update account set balance = balance - 500 where name='张三';
-- 李四账号+500
update account set balance = balance + 500 where name='李四';

COMMIT; -- 提交事务
```

开启事务后，所有的操作都在临时日志文件中，`commit` 操作是将临时日志中的内容写到数据库的存储引擎中，所以在提交事务之前，是不会对数据库中的内容进行修改的。而 `rollback` 则是清空临时日志文件，之前没有进行提交的内容全部清除。

事务的步骤可以简述为下面几步

- 客户端连接数据库服务器，创建连接时创建此用户临时日志文件 
- 开启事务以后，所有的操作都会先写入到临时日志文件中 
- 所有的查询操作从表中查询，但会经过日志文件加工后才返回 
- 如果事务提交则将日志文件中的数据写到表中，否则清空日志文件

事务的提交分为自动提交和手动提交，在 `MySQL` 命令行的默认设置下，事务都是自动提交的，即执行 `SQL` 语句后就会马上执行 `COMMIT` 操作。因此要显式地开启一个事务务须使用命令 `BEGIN` 或 `START TRANSACTION`，或者执行命令 `SET AUTOCOMMIT=0`，用来禁止使用当前会话的自动提交。

- **SET AUTOCOMMIT=0** 
  - 禁止自动提交
- **SET AUTOCOMMIT=1** 
  - 开启自动提交

### 事务的特性

事务的四大特性 ACID

- 原子性(Atomicity)
  - 每个事务都是一个整体，不可再拆分，事务中所有的 `SQL` 语句要么都执行成功， 要么都失败。
- 一致性(Consistency)
  - 事务在执行前数据库的状态与执行后数据库的状态保持一致。如：转账前 2 个人的 总金额是 2000，转账后 2 个人总金额也是 2000 
- 隔离性(Isolation)
  - 事务与事务之间不应该相互影响，执行时保持隔离的状态。 
- 持久性(Durability)
  - 一旦事务执行成功，对数据库的修改是持久的。就算关机，也是保存下来的。 

这篇文章[MySQL事务：ACID特性的实现原理总结分析](https://zhuanlan.zhihu.com/p/60723043)详细讲解了 MySQL 的事务，所以在这里我不多做介绍，因为讲的没人家好。

### 事务的隔离级别

事务在操作时的理想状态是所有的事务之间保持隔离，互不影响。但是因为并发操作，多个用户同时访问同一个数据时，可能引发并发访问的问题，如

- 脏读 
  - 一个事务读取到了另一个事务中尚未提交的数据
  - 比如李四向张三转了 500 块，但是没有提交，这时张三读取数据，发现已经到账 500 块，跟李四说到账了，这时李四进行 `roll back`
- 不可重复读 
  - 一个事务中两次读取的数据内容不一致，要求的是一个事务中多次读取时数据是一致的，这是事务 `update` 时引发的问题
  - 两次查询输出的结果不同，到底哪次是对的? 不知道以哪次为准。 有的时候这不是一个问题，当然是后面的为准。但是我们可以考虑这样一种情况，比如银行程序需要将查询结果分别输出到电脑屏幕和发短信给客 户，结果在一个事务中针对不同的输出目的地进行的两次查询不一致，导致文件和屏幕中的结果不一致，银行工作 人员就不知道以哪个为准了。 
  - 脏读与不可重复读的区别在于：前者读到的是其他事务未提交的数据，后者读到的是其他事务已提交的数据
- 幻读
  - 一个事务中两次读取的数据的数量不一致，要求在一个事务多次读取的数据的数量是一致的，这是 `insert` 或 `delete` 时引发的问题
  - 幻读是事务非独立执行时发生的一种现象。例如事务 T1 对一个表中所有的行的某个数据项做了从 "1" 修改为 "2" 的操作，这时事务 T2 又对这个表中插入了一行数据项，而这个数据项的数值还是为 "1" 并且提交给数据库。而操作事务 T1 的用户如果再查看刚刚修改的数据，会发现还有一行没有修改，其实这行是从事务 T2 中添加的，就好像产生幻觉一样，这就是发生了幻读。
  - 幻读和不可重复读都是读取了另一条已经提交的事务(这点就脏读不同)，所不同的是不可重复读查询的都是同一个数据项，而幻读针对的是一批数据整体(比如数据的个数)。

MySQL数据库有四种隔离级别 

- `read uncommitted`
  - 上面三种问题都有可能发生
- `read committed`
  - 不可能发生脏读，因为只有读到已提交的数据
- `repeatable read`
  - 可能发生幻读
  - 这时 MySQL 的默认隔离级别
- `serializable`
  - 所有的问题都不会发生

上面的级别最低，下面的级别最高，隔离级别越高，性能越差，安全性越高。在 MySQL 数据库中设置事务的隔离级别

```sql
set [glogal | session] transaction isolation level 隔离级别名称;
set tx_isolation='隔离级别名称';
```

## 权限管理

我们现在默认使用的都是 root 用户：超级管理员，拥有全部的权限。但是一个公司里面的数据库服务器上面可能同时运行着很多个项目的数据库。所以我们应该可以根据不同的项目建立不同的用户，分配不同的权限来管理和维护数据库。

### 创建用户

```sql
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码'; 
```

- '用户名' 
  - 将创建的用户名 
- '主机名' 
  - 指定该用户在哪个主机上可以登陆，如果是本地用户可用 localhost，如果想让该用户可以 从任意远程主机登陆，可以使用通配符% 
- 该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器 

```sql
#创建 user1 用户，只能在 localhost 这个服务器登录 mysql 服务器，密码为 123 
create user 'user1'@'localhost' identified by '123'; 
#创建 user2 用户可以在任何电脑上登录 mysql 服务器，密码为 123 
create user 'user2'@'%' identified by '123'; 
```

创建的用户名都在 mysql 数据库中的 user 表中可以查看到，密码经过了加密。 

### 授予权限

```sql
GRANT 权限 1, 权限 2... ON 数据库名.表名 TO '用户名'@'主机名'; 
```

- `GRANT…ON…TO` 
  - 授权关键字 
- 权限
  - 授予用户的权限，如 `CREATE、ALTER、SELECT、INSERT、UPDATE` 等。如果要授 予所有的权限则使用 `ALL` 
- 数据库名.表名 
  - 该用户可以操作哪个数据库的哪些表。如果要授予该用户对所有数据库和表的相应操作 权限则可用 \* 表示，如 \*.\* 
- '用户名'@'主机名' 
  - 给哪个用户授权

```sql
#给 user1 用户分配对 test 这个数据库操作的权限：创建表，修改表，插入记录，更新记录，查询 
grant create,alter,insert,update,select on test.* to 'user1'@'localhost'; 
 
#给 user2 用户分配所有权限，对所有数据库的所有表  
grant all on *.* to 'user2'@'%'; 
```

### 撤销权限

```sql
REVOKE  权限 1, 权限 2... ON 数据库.表名 from '用户名'@'主机名'; 
```

- `REVOKE…ON…FROM `
  - 撤销授权的关键字 
- 权限 
  - 用户的权限，如 `CREATE、ALTER、SELECT、INSERT、UPDATE` 等，所有的权限则使用 `ALL` 
- 数据库名.表名 
  - 对哪些数据库的哪些表，如果要取消该用户对所有数据库和表的操作权限则可用\*表 示，如\*.\* 
- '用户名'@'主机名' 
  - 给哪个用户撤销  

```sql
#撤销 user1 用户对 test 数据库所有表的操作的权限 
revoke all on test.* from 'user1'@'localhost'; 
```

### 查看权限

```sql
SHOW GRANTS FOR '用户名'@'主机名'; 
```

```sql
SHOW GRANTS FOR 'root'@'localhost';
```

### 删除用户

```sql
DROP USER '用户名'@'主机名'; 
```

### 修改管理员密码

如果忘了原来的管理员密码

1. `cmd --> net stop mysql` 停止 mysql 服务
   - 需要管理员运行该 cmd
2. 使用无验证方式启动 mysql 服务：`mysqld --skip-grant-tables`
3. 打开新的 cmd 窗口,直接输入 mysql 命令，敲回车。就可以登录成功
4. `use mysql;`
5. `update user set password = password('你的新密码') where user = 'root';`
6. 关闭两个窗口
7. 打开任务管理器，手动结束 mysqld.exe 的进程
8. 启动 mysql 服务
9. 使用新密码登录。

如果记得原来的密码

```sql
mysqladmin -uroot -p password 新密码 #需要在未登陆 MySQL 的情况下操作，新密码不需要加上引号。 
```

### 修改普通用户密码 

```sql
set password for '用户名'@'主机名' = password('新密码'); 
```

## 参考链接

- [数据库隔离级别，每个级别会引发什么问题](https://www.cnblogs.com/whu-2017/p/9293083.html)
- [MySQL事务：ACID特性的实现原理总结分析](https://zhuanlan.zhihu.com/p/60723043)
- [MySQL数据查询之多表查询](https://www.cnblogs.com/bypp/p/8618382.html)
- [MySQL插入中文错误](https://blog.csdn.net/ch717828/article/details/41357431)


