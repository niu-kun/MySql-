# MySql学习笔记

## 1. MySql [参考手册](http://dev.mysql.com/)

## 2. 数据库常用操作（增删改查）

### 2.1 增

> 在数据库服务器中，创建数据库

* ```CREATE DATABASE database_name;```

> 在数据库服务器中，创建数据表

```Sql
USE student;
CREATE TABLE table_name(
    name VARCHAR(20),
    sex CHAR(1),
    birth DATE);
```

> 往数据表中添加数据记录

```Sql
INSERT INTO table_name
VALUES ('张三','男','2010-02-21');
INSERT INTO table_name
VALUES ('李四','男','2009-01-21');
```

### 2.2 删

> 删除数据

```DELETE FROM table_name WHERE NAME='牛坤';```

> 删除数据表

```DROP TABLE table_name;```

> 删除数据库

```DROP DATABASE database_name;```

### 2.3 改

> 修改数据

```UPDATE table_name SET name='王五' WHERE name='张三'```

> 修改表名

```ALTER TABLE old_table_name RENAME TO new_table_name```

> 修改数据库名字

* <https://blog.csdn.net/gruhgd/article/details/84065218>

> 修改列的数据类型

```ALTER TABLE table_name MODIFY COLUMN column_name new_type;```

> 修改列的名字

```ALTER TABLE old_table_name CHANGE COLUMN old_column_name new_column_name new_column_type```

### 2.4 查

> 查询所有数据库

* ```SHOW DATABASES;```

> 使用具体数据库，并查看该数据库所有表格

* ```USE database_name;```
* ```SHOW TABLES```

> 查看创建好的数据表的结构

```USE table_name;```
```DESCRIBE table_name;```

> 查询具体表

* ```SELECT * FROM table_name;```
* ```SELECT column_name FROM table_name;```
* ```SELECT column_name FROM table_name WHERE column_name=1;```

> MySql版本查询

* ```SELECT VERSION();```
* ```STATUS; 或者 /s;```

* 查看mysql默认字符集
* ```SHOW VARIABLES LIKE "character%";```

### 2.4 退

> 退出数据库

* ```QUIT;```
* ```EXIT;```

### 2.5 常用数据类型

> MySql常用数据类型

* 支持多种数据类型，大致分为三类：
* 数值类型
* ![数值类型](数值类型.jpg)
* 日期/时间类型
* ![日期/时间类型](日期时间类型.jpg)
* 字符串（字符）类型
* ![字符串（字符）类型](字符字符串类型.jpg)

> [MySql的默认字符集设置](https://blog.csdn.net/gh_love/article/details/98097234)

## 3. MySql建表约束

> 主键约束（可设置多个字段为主键）（**不可重复！不可为空！**）。
>> 联合主键,即如果有多个主键，只要其中一个不同即可。  
>> 能够唯一确定一张表中的记录，也就是我们通过某个字段添加约束，就可以使得字段不重复且不为空。  
>> 如果建表的时候，忘记指定主键？

```Sql
CREATE TABLE user(
    id int PRIMARY KEY,
    name varchar(20)
);

CREATE TABLE user2(
    id int,
    name varchar(20),
    password varchar(20),
    PRIMARY KEY(id,name)
);

CREATE TABLE user4(
    id int,
    name varchar(20)
);
/* 添加主键 */
ALTER TABLE user4 add PRIMARY KEY(id,name);
/* 删除主键 */
ALTER TABLE user4 DROP PRIMARY KEY;
/* 修改主键 */
ALTER TABLE user4 MODIFY id int PRIMARY KEY;
```

>> 删除含有外键约束的主键

<https://blog.csdn.net/CSDN_Mr_H/article/details/90317043>

> 自增约束

```Sql
CREATE table user3(
    id int PRIMARY AUTO_INCREMENT,
    name varchar(20)
);

INSERT INTO user3 VALUES(NULL,'小米');
INSERT INTO user3 VALUES(3,'大米');
INSERT INTO user3 VALUES(8,'大米');
INSERT INTO user3 VALUES(NULL,'大米');
INSERT INTO user3 (name)VALUES('红枣');
INSERT INTO user3 (name)VALUES('玉米');
```

![自增测试结果](自增测试结果.jpg)

> 唯一约束
>> 约束修饰的字段的值不可以重复。

```Sql
create table user5(
    id int,
    name varchar(20)
);

/* 添加唯一约束 三种方法*/
ALTER TABLE user5 ADD UNIQUE(name);

ALTER TABLE user5 MODIFY name varchar(20) unique;


CREATE TABLE user6(
    id int,
    name varchar(20),
    unique(id)
);

/* 删除唯一约束 */
ALTER TABLE user6 drop INDEX id;
```

> 非空约束
>> 修饰的字段不能为空。

```Sql
CREATE TABLE user7(
    id int,
    name varchar(20) NOT NULL
);
```

> 默认约束
>> 当插入字段值的时候，如果没有传值使用默认值。

```Sql
CREATE TABLE user8(
    id int,
    bane varchar(20),
    age int DEFAULT 18
);

```

> 外键约束
>> 涉及到两个表：父表，子表。  
>> 主表，副表。  
>> 主表中没有的数据，副表不可以使用。  
>> 主表中的记录被副表引用，是不可以删除的。

```Sql
/* 班级表 */
CREATE TABLE classes(
    id int PRIMARY KEY,
    name VARCHAR(20)
);

/* 学生表 */
CREATE TABLE students(
    id int PRIMARY KEY,
    name VARCHAR(20),
    class_id int,
    foreign key(class_id) references classes(id)
);

/* 往班级表插入数据 */
INSERT INTO classes values(1,'一班');
INSERT INTO classes values(2,'二班');
INSERT INTO classes values(3,'三班');
INSERT INTO classes values(4,'四班');

/* 往学生表插入数据 */
INSERT INTO students VALUES(2018,'张三',1);
INSERT INTO students VALUES(2019,'张三',2);
INSERT INTO students VALUES(2020,'张三',3);
INSERT INTO students VALUES(2021,'张三',4);

/* 主表中没有的数据，副表不可以使用 */
INSERT INTO students VALUES(2022,'张三',5);

/* 主表中的记录被副表引用，是不可以删除的。 */
DELETE FROM classes WHERE id=1;

```

## 4. 数据库的三大设计范式

### 4.1 第一范式

> 数据表中所有字段都是不可分割的原子值。
> 范式，设计的越详细，对于某些操作越好，但是不一定都是好处。

```Sql

/* 字段直还可以继续拆分，不满足第一范式 */
CREATE TABLE student2(
    id INT PRIMARY KEY,
    name VARCHAR(20),
    address VARCHAR(30)
);
INSERT INTO student2 VALUES(1,'张三','中国四川省成都市成电');
INSERT INTO student2 VALUES(2,'张四','中国四川省成都市成电');
INSERT INTO student2 VALUES(3,'张五','中国四川省成都市成电');

/* 满足第一范式例子 */
CREATE TABLE student3(
    id INT PRIMARY KEY,
    name VARCHAR(20),
    country VARCHAR(30),
    province VARCHAR(30),
    city VARCHAR(30),
    details VARCHAR(30)
);
INSERT INTO student3 VALUES(1,'张三','中国','四川省','成都市','成电');
INSERT INTO student3 VALUES(2,'李四','中国','四川省','成都市','成电');
INSERT INTO student3 VALUES(3,'王五','中国','四川省','成都市','成电');
```

### 4.2 第二范式

> 必须满足第一范式的前提下，第二范式要求，除主键外的每一列必须完全依赖于主键。  
> 如果不依赖于，只可能发生在联合主键的情况下。

```Sql

/* 不满足第二范式 */
CREATE TABLE myOrder(
    product_id int,
    customer_id int,
    product_name varchar(20),
    customer_name varchar(20),
    primary key(product, customer_id)
);

/* 满足第二范式 拆分成三个表*/
CREATE TABLE myOrder(
    order_id int primary key,
    product_name int,
    customer_id int
);

CREATE TABLE product(
    id int primary key,
    name varchar(20)
);

CREATE TABLE customer(
    id int primary key,
    name varchar(20)
);


```

### 4.3 第三范式

> 必须先满足第二范式，除开主键列的其他列不能有传递依赖关系。

```Sql

/* 不满足第三范式 */
CREATE TABLE myOrder(
    order_id int primary key,
    product_name int,
    customer_id int,
    customer_phone varchar(15)
);

/* 满足第三范式 */
CREATE TABLE myOrder(
    order_id int primary key,
    product_name int,
    customer_id int
);

CREATE TABLE customer(
    id int primary key,
    name int,
    phone varchar(15)
);

```

## 5. MySql查询练习

> 创建学生表、教师表、课程表、成绩表。

```Sql

    /* 学生表 */
    CREATE TABLE student(
        SNo varchar(20) primary key,
        SName varchar(20) not null,
        SSex char(1) not null,
        SBirth datetime,
        SClass varchar(15)
    );

     /* 教师表 */
    CREATE TABLE teacher(
        TNo varchar(20) primary key,
        TName varchar(20) not null,
        TSex char(1) not null,
        TBirth datetime,
        TProf  varchar(20) not null,
        TDepart varchar(15) not null
    );

    /* 课程表 */
    CREATE TABLE course(
        CNo varchar(20) primary key,
        CName varchar(20) not null,
        CTNo varchar(20) not null,
        foreign key(CTNo) references teacher(TNo)
    );

    /* 成绩表 */
    CREATE TABLE score(
        SSNo varchar(20) primary key,
        SCNo varchar(20) not null,
        SScore decimal not null,
        foreign key(SSNo) references student(SNo),
        foreign key(SCNo) references course(CNo)
    );

    /* 学生表添加数据 */
    insert into student values('1','张一','男','1998-01-10','1001');
    insert into student values('2','张二','女','1999-01-10','1002');
    insert into student values('3','张三','男','1995-01-10','1003');
    insert into student values('4','张四','女','1993-01-10','1004');
    insert into student values('5','张五','女','1998-01-10','1005');

    /* 教师表添加数据 */
    insert into teacher values('2001','兆一','男','1965-02-20','讲师','计算机系');
    insert into teacher values('2002','兆二','女','1962-02-20','教授','机电系');
    insert into teacher values('2003','兆三','男','1961-02-20','教授','计算机系');
    insert into teacher values('2004','兆四','女','1966-02-20','副教授','音乐系');

    /* 课程表 */
    insert into course values('3-102','数据结构与算法','2001');
    insert into course values('3-245','CSharp入门','2002');
    insert into course values('6-1662','高数','2003');
    insert into course values('8-682','英语','2004');

    /* 成绩表 */
    insert into score values('1','3-102','99');
    insert into score values('2','3-245','96');
    insert into score values('3','6-1662','66');
    insert into score values('4','8-682','58');
    insert into score values('5','3-102','88');
```

> 查询练习

```Sql

/* 1. 查询教师所有单位不重复的depart列 */
/* distinct排除重复 */
select distinct TDepart from teacher;

/* 2. 查询score表成绩在60到80之间的记录 */
/* 查询区间 between ... and ... */
/* 查询区间 ...>... and ...<... 运算符比较 */
select * from score where SScore between 60 and 80;
select * from score where SScore>50 and SScore<80;

/* 3. 查询score表中指定成绩88,66的记录 */
/* in 关键字 同字段即同列*/
select * from score where SScore in(88,66);

/* 4. 查询student表中“1001”班 或 性别为女的同学记录*/
/* or 关键字 不同字段即不同列 */
select * from student where SClass='1001' or SSex='女';

/* 5. 以SClass降序查询student表的所有记录 默认升序*/
/* order by ... desc */
select * from student order by SClass desc;
select * from student order by SClass asc;

/* 6. 以SCNo升序，SScore降序查询score表的所有记录 */
/* order by ... desc, ... asc */
select * from score order by SCNo asc, SScore desc;

/* 7. 查询1001班学生人数 */
/* count(*) */
select count(*) from student where SClass='1001';

/* 8. 查询score表中最高分学生的学生号和课程号 */
/* max 复合语句 */
select SSNo,SCNo from score where SScore=(select max(SScore) from score);

/* 正常操作步骤 */

select max(SSCore) from score;
select SSNo,SCNo from score where SScore=99;

/* 9. 查询每门课的平均成绩 */
/* avg */
/* 单个查询 */
select avg(SScore) from score where SCNo='3-102';
/* 组合查询  group by*/
select SCNo,avg(SScore) from score group by SCNo;

/* 11. 查询score表中至少有2名学生选修的并以3开头的课程的平均分 */
select SCNo,avg(SScore) from score group by SCNo having count(SCNo)>=2 and SCNo like '3%';

/* 12. 查询所有学生的 SName、SCNo 和 SScore 列 */
/*  */
select SName,SCNo,SScore from student,score where student.SNo=score.SSNo;

/* 13. 查询所有学生的SSNo、CName和SScore列 */
/*  */
select SSNo,CName,SScore from course,score where score.SCNo=course.CNo;
/* 14. 查询所有学生的SName、CName和SScore列*/
/*  */
select SName,CName,SSCore from student,course,score where student.SNo=Score.SSNo and Score.SCNo=Course.CTNo;
/* 15. 查询"1001"班学生每门课的平均分 */
/*  */
select SCNo,avg(SSCore) from score where SSNo in(select SNo from student where SClass='1001') group by SCNo;
```

## 6. SQL 的四种连接查询

> 内连接

* inner join 或者 join  
  通过表中某个字段相对，查询出相关记录数据，不需要创建外键。

> 外连接

* 左连接  
  left join 或者 left outer join  
  把左边表数据全部取出来，右边的表数据有相等的，则显示。没有补NULL。

* 右连接  
  right join 或者 right outer join  
  把右边表数据全部取出来，左边表有相等的则显示。没有则补NULL。
* 完全外连接  
  full join 或者 full outer join

```Sql
CREATE TABLE person(
    id int,
    name varchar(20),
    cardId int
);

CREATE TABLE card(
    id int,
    name varchar(20)
);

insert into person values(1,'张一',1001);
insert into person values(2,'李二',1002);
insert into person values(3,'王三',1003);
insert into person values(4,'李四',1004);
insert into person values(5,'赵五',1005);

insert into card values(1001,'建行');
insert into card values(1002,'中行');
insert into card values(1003,'农行');
insert into card values(1004,'汇丰行');

/* inner join 查询 （内连接）*/
select * from person inner join card on person.cardId=card.id;

/* left join（左外连接） */
select * from person left join card on person.cardId=card.id;
/* right join（右外连接） */
select * from person right join card on person.cardId=card.id;
/* full join（完全外连接） 实现方式*/
MySql不支持全外来连接。

select * from person left join card on person.cardId=card.id
union
select * from person right join card on person.cardId=card.id;
```

## 7. MySql 事务

* MySql 中，事务是一个不可分割的工作单元。事务能够保证一个业务的完整性。  
* 设置 MySql 自动提交为 false，并查看是否设置成功。  
  ```set autocommit=0;```  
  ```select @@autocommit;```  
* 设置成功后可以用 **rollback** 实现回滚。  
  ```rollback;```
* 如果确认提交，则输入 **commit** 即可。
  ```commit;```
