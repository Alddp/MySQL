## 数据库操作

### 创建数据库

```mysql
CREATE DATABASE IF NOT EXISTS SchoolDB1
DEFAULT CHARACTER SET gb2312
DEFAULT COLLATE gb2312_chinese_ci;
```

 

### 查看数据库

```mysql
SHOW databases;
```

 

### 打开数据库

```mysql
use databasesname;
```



### 删除数据库（DROP）

```mysql
DROP DATABASE IF EXISTS databasename;
```

 

### 修改数据库字符集校队规则

```mysql
ALTER DATABASE schoolDB1
    DEFAULT CHARACTER SET utf8
    DEFAULT COLLATE utf8_general_ci;
```

 

### 设置MySQL字符集

#### 查看系统字符集参数

```mysql
show variables like'character';
```

 

#### 修改系统字符集

```mysql
set character_set_server=’gb2312’;
 set character_set_database=’gb2312’;
```

 

查看是否修改成功：

```mysql
status;
```

 

修改数据库字符集校对规则:

```mysql
ALTER DATABASE schoolDB1
    DEFAULT CHARACTER SET utf8
    DEFAULT COLLATE utf8_general_ci;
```

 

## 表操作

### 创建表

```mysql
CREATE TABLE IF NOT EXISTS Grade
(
    gradeId   INT(4)      NOT NULL COMMENT '年级编号',
    gradeName VARCHAR(50) NOT NULL COMMENT '年级名称'
);
```



### 约束

#### 非空约束

创建时设置非空约束

```mysql
CREATE TABLE grade2
(
    gradeId   INT(4) NOT NULL COMMENT '年级编号',
    gradeName VARCHAR(50) COMMENT '年级名称'
);
```

 

#### 唯一约束

创建时设置唯一约束

```mysql
identityCard VARCHAR(18) UNIQUE COMMENT '身份证号'
```

 

#### 主键约束

创建表时设置主键约束

```mysql
gradeId INT(4) NOT NULL  COMMENT '年级编号',
PRIMARY key（列名,列名）;
```

 

#### 默认值约束

创建表时设置默认约束

```mysql
address VARCHAR(255) DEFAULT '地址不详' COMMENT '地址'
```

 

#### 检查约束

创建表时设置检查约束

```mysql
classHour INT
CHECK (classHour >= 0) COMMENT '学时';
```

 

#### 自增(auto_increment)

```mysql
create table if not exists a
 (
   id int auto_increment primary key
 );
```

 

### 管理数据表

#### 查看数据表

查看所有表:

```mysql
SHOW TABLES;
```

 

查看单个表:

```mysql
DESC grade;
```

 

#### 复制数据表

复制表(like复制表结构 as复制表数据):

```mysql
CREATE TABLE grade91 LIKE grade;
```

 

```mysql
CREATE TABLE result91 AS (SELECT * FROM grade);
```

 

#### 修改数据表名

```mysql
RENAME TABLE result91 TO result92;
```

 

#### 修改列的数据类型:

```mysql
ALTER TABLE grade
    MODIFY gradeName CHAR(20);
```

 

#### 修改数据表结构

删除列:

```mysql
ALTER TABLE grade
    DROP score;
```

 

#### 删除数据表（DROP）

```mysql
DROP TABLE IF EXISTS grade2;
```

 

## 数据操作

### 查询（SELECT）

#### 单表查询

##### 使用select语句进行查询

###### select 语句的格式和功能(筛选语句有分先后)

```mysql
select *
 from schooldb
 [where]--
 [group by]--
 [having]--
 [order by]--;
```

 

###### 查询所有数据行和列

```mysql
select * from grade;
```

 

###### 查询指定的列

```mysql
select gradeId
 from grade;
```

 

###### 改变查询结果的列标题(AS)

```mysql
select gradeId as '年级'
 from grade;
```

 

###### 限制查询结果返回记录的行数(LIMIT)

```mysql
select subjectId,studentResult
 from result
 order by studentResult
 limit 5;
```

 

###### 消除查询结果的重复行(DISTINCT)

```mysql
select distinct gradeId as '年级编号'
 from student;
```

 

###### 查询中使用计算列(使用+、-、*、/)

```mysql
select subjectName   as '课程名称'
   , classHour    as '原课时'
   , classHour * 1.1 as '增加%10后课时'
 from subject;
```

 

###### 使用where子句查询部分行

```mysql
select *
 from student
 where gradeId = 1;
```

 

###### 查询空值

```mysql
select *
 from student
 where email is null;
```

 

##### 使用order by 子句进行查询排序

```mysql
"ascending"（ASC升序）, "descending"（DESC降序）
```



###### order by 子句的语法格式

```mysql
SELECT * FROM result;
 SELECT subjectId AS '课程编号'
 FROM result
 ORDER BY subjectId DESC;
```



###### 子句功能

##### 5.2使用函数查询数据

###### 字符串函数

###### 日期函数

###### 数学函数

#### 5.3模糊查询

##### 通配符查询和模糊查询

##### 使用like进行模糊查询

```mysql
SELECT GradeId AS '年级编号', *COUNT*(*) AS '安徽地区分年级人数'
 FROM student
 WHERE address LIKE '安徽省%'
 GROUP BY GradeId;
```

 

##### 使用between在某个范围内进行查询

```mysql
select *
 from result
 where studentResult between 70 and 85;
 -- 初值必须小于终值
```

 

##### 使用IN在列举值内进行查询

```mysql
select *
 from subject
 where GradeId in (2, 3, 4);
```

 

#### 5.4使用聚合函数进行数据的统计

##### SUM

```mysql
SELECT studentno AS '学号', *SUM*(studentResult) AS '总分'
 FROM result
 GROUP BY studentno
 ORDER BY *SUM*(studentResult) DESC;
```

 

##### AVG

```mysql
SELECT *AVG*(studentResult)
 FROM result
 WHERE examDate = '2020-01-07'
  AND subjectId = 2;
```

 

##### MAX/MIN

```mysql
SELECT *MAX*(studentResult)
 FROM result
 WHERE examDate = '2020-01-07'
  AND subjectId = 2;
```

 

##### COUNT

```mysql
SELECT *COUNT*(studentno) AS '总人数'
 FROM student;
 SELECT *
 FROM result;
```

 

#### 分组查询

##### 使用GROUP BY进行分组查询

```mysql
SELECT GradeId AS '年级',sex AS '性别',*COUNT*(*) AS '人数'
 FROM student
 GROUP BY GradeId;
```

 

##### 使用HAVING子句进行分组筛选

```mysql
SELECT studentno AS '学号', *AVG*(studentResult) AS '平均分'
 FROM result
 GROUP BY studentno
 HAVING *AVG*(studentResult)>=60
 ORDER BY *AVG*(studentResult)DESC;
```

 

##### 子查询

###### 简单查询

###### IN字查询

###### EXISTS子查询

 

#### 多表查询

### 插入数据(INISERT)

#### 插入单条记录

```mysql
insert into grade (gradeId, gradeName)
values (1, 'S1');
```

 

#### 插入多条记录

1为所有字段添加值:

```mysql
INSERT INTO Student
VALUES ('G1263201', '1', '王子洋', '男', 5, '18655290000', '安徽省蚌埠市', '1993-08-07', 'wzy@163.com',
        '340423199308070000'),
       ('G1263382', '1', '张琪', '女', 5, '15678090000', '安徽省合肥市', '1993-05-07', 'zhangqi@126.com',
        '340104199305070000'),
       ('G1263458', '1', '项宇', '男', 5, '18298000000', '地址不详', '1992-12-10', 'xiangyu@163.com',
        '340881199212100000');
```

 

2选择字段添加值:

```mysql
insert into Reader(rId, rName, lendNum)
 values ('001', 'zhangYongwei', 1),
    ('002', 'zhangDawei', 2);
```

 

#### 插入查询结果

(用于复制数据)

```mysql
insert into grade_try
select * from grade
where gradeId = 2;
```mysql

 

### 修改数据(UPDATE)

#### 修改符合条件的数据

update student set email='未知@'
 where email is null;

 

### 删除数据(DELECT、TRUNCATE)

#### 删除一行或多行数据(DELECT)

```mysql
delete from result
where studentResult=80;
```

 

#### 清空数据表(TRUNCATE)

```mysql
truncate table result;
```

### 运算符

 

## 其他函数

#### 当前时间(*now*(),*curdate*())

```mysql
select *now*();
```

![图形用户界面, 文本, 应用程序  描述已自动生成](file:///C:/Users/19067/AppData/Local/Temp/msohtmlclip1/01/clip_image002.gif)

```mysql
select *curdate*();
```

 

![图形用户界面, 文本, 应用程序, 聊天或短信  描述已自动生成](file:///C:/Users/19067/AppData/Local/Temp/msohtmlclip1/01/clip_image004.gif)

#### 平方根(*sqrt()*)

```mysql
select *sqrt*(9);
```

 

#### 当前用户(*user()*)

```mysql
select *user*() as '查询当前用户'
 from student;
```

![图形用户界面, 应用程序  描述已自动生成](file:///C:/Users/19067/AppData/Local/Temp/msohtmlclip1/01/clip_image006.gif)
