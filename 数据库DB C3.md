# SQL四大类



| 类型  | 功能      | 常见关键字                |
| --- | ------- | -------------------- |
| DDL | 定义数据库对象 | CREATE、ALTER、DROP    |
| DML | 修改数据    | INSERT、UPDATE、DELETE |
| DQL | 查询数据    | SELECT               |
| DCL | 权限控制    | GRANT、REVOKE（一般不考）   |
*考试一般就是*  **DDL+DML+SELECT**

# PART 1. CREATE TABLE
基本模式定义（create table）
这个就是建表 ^51c1db

eg. 学生表，有
- 学号
- 姓名
- 年龄
- 专业

SQL写法
```SQL
CREATE TABLE Student(
	Son CHAR(10),
	Snake VARCHAR(20),
	Age INT,
	Major VARCHAR(30)
);
```

### 主键怎么定义？

eg. 学号作为主键

```SQL
CREATE TABLE Student(
	Son CHAR(10) PRIMARY KEY,
	Snake VARCHAR(20),
	Age INT
);
```

意思：
**Sno是主码**
- 不能重复
- 不能为空

### 外键怎么定义？

**外键 = 本表里的某个字段，去引用另一张表的主键**

eg. 假设学校数据库有三张表：
1. 学生表Student（主键Sno）
	- Sno
	- Sname
2. 课程表Course（主键Cno）
	- Cno
	- Cname
3. 选课表SC
	- Sno
	- Cno
	- Grade
	其中， SC.Sno 来自 Student.Sno
		 SC.Cno来自Course.Cno

eg. 外键SQL怎么写？

Step 1. 建学生表
```SQL
CREATE TABLE Student (
	Sno CHAR(10) PRIMARY KEY,
	Snake VARCHAR(20)
);
```
Step 2. 建课程表
```SQL
CREATE TABLE Course (
	Cno CHAR(10) PRIMARY KEY,
	Name VARCHAR(20)
);
```

Step 3. 建选课表

```SQL
CREATE TABLE SC (
	Sno CHAR(10),
	Cno CHAR(10),
	Grade INT,
	FOREIGN KEY (Sno) REFERENCES Student(Sno),
	FOREIGN KEY (Cno) REFERENCES Course(Cno)
);
```
*外键字段和被引用字段通常名字一样，但不是必须一样*

比如选课表也可以：
```SQL
CREATE TABLE SC(
	StudentID CHAR(10),
	CourseID CHAR(10),
	Grade INT,
	FOREIGN KEY (StudentID) REFERENCES Student(Sno),
	FOREIGN KEY (CourseID) REFERENCES Course(Cno)
);
```

##### 外键一定是主键吗？
	**外键值本表中不一定是主键**
	那SC表的主键是什么？
	一般是（Sno，Cno）

# Part 2. 对表操作
##### 删除表：
```SQL
DROP TABLE Student;
```
整个Student表没了，带着数据一起删
##### 修改表：
eg. 增加一个电话属性
```SQL
ALTER TABLE Student
ADD Phone CHAR(11);
```
eg. 删除一列：
```SQL
ALTER TABLE Student
DROP COLUMN Phone;
```
eg. 修改字段：
```SQL
ALTER TABLE Student
MODIFY Age SMALLINT;
```
# PART 3. 对数据的操作（重点）

##### INSERT
新增一条数据
```SQL
INSERT INTO Student
VALUES(‘001’,’张三’,20);
```
也可以
```SQL
INSERT INTO Student(Sno,Sname)
VALUES(‘001’,’张三’);
```
(只插两列)

##### DELETE
删除数据
eg. 删学号001的学生
```SQL
DELETE
FROM Student
WHERE Sno=‘001’;
```
**DELETE删除的是行**
如果是
```SQL
DELETE FROM Student;
```
没有where的话，那就是“全部删光”了
但是，表还在，不要和DROP TABLE 搞混了

##### UPDATE
修改
```SQL
UPDATE Student
SET Age=21
WHERE Sno=‘001’;
```

题目，建立学生表，包括学号姓名年龄，其中学号是主键
```SQL
CREATE TABLE(
	Sno CHAR(10) PRIMARY KEY,
	SName VARCHAR(20),
	Age INT
);
```
删除学号001
```SQL
DELETE
FROM Student
WHERE Sno=‘001’
```
修改001的年龄为22
```SQL
UPDATE Student
SET Age=22
WHERE Sno=‘001’
```
插入*001 张三 20*
```SQL
INSERT INTO Student
VALUES(‘001’,’张三‘，20)；
```

# SQL查询语句知识点

^1021ef

#### Part 1. 基础模板
```SQL
SELECT ...
FROM ...
WHERE ...
```
- 查询指定列
```SQL
SELECT name
FROM student;
```
- 查询所有列
```SQL
SELECT *
FROM student;
```
- DISTINCT 去重
```SQL
SELECT DISTINCT dept_name
FROM student;
```

#### Part 2. WHERE
- 比较运算
```SQL
=
<> or != //不等于
>
<
>=
<=
```
- 逻辑运算
```SQL
AND
OR
NOT
```
- 集合
```SQL
IN(...)
```
- 区间
```SQL
BETWEEN ... AND ...
```
- 空值
```SQL
IS NULL
IS NOT NULL
```

#### Part 3. LIKE
```Plain text
%
任意长度字符
```
```Plain text
-
任意一个字符
```

#### Part 4. ORDER BY
```SQL
ORDER BY salary DESC
按照salary的字段降序排列
```
```SQL
ORDER BY salary ASC
升序排列（默认的方式）
```

#### Part 5. 聚集函数
```SQL
COUNT()
SUM()
AVG()
MAX()
MIN()
```

#### Part 6. GROUP BY

**必须理解：先分组，再统计**

eg. 每个学院平均工资
```SQL
SELECT dept_name, AVG(salary)
FROM instructor
GROUP BY dept_name;
```

#### Part 7. HAVING

必须理解：**HAVING是筛选组**
例如：
平均工资超过70000的学院：
```SQL
HAVING AVG(salary)>70000
```

#### Part 8. 多表查询

这是Chapter 4的基础

基础：
```SQL
FROM A,B
WHERE A.id=B.id
```
以及：
```SQL
JOIN
ON
```

#### Part 9. 集合运算

必须认识：
```SQL
UNION        //去重并集
INTERSECT    //交集
EXCEPT       //差集
```

#### Part 10. 修改数据库
```SQL
INSERT
DELETE
UPDATE
```

#### Part 11. 建表
```SQL
CREATE TABLE
ALTER TABLE
DROP TABLE
```

#### 常见SQL错误

- 使用 `GROUP BY` 之后，SELECT 后面一般只能出现两类东西：
```
1. GROUP BY 后面的字段
2. 聚集函数
```
---
- WHERE 和HAVING用反
	**WHERE** 是在**分组之前**筛选一行一行的数据
	但是**AVG(salary)**是分组之后才能算出来的
- WHERE 筛选原始行
- HAVING 筛选分组后的结果
eg. 查询工资大于 70000 的老师
```SQL
SELECT name, salary
FROM insrtucontorted
WHERE salary > 70000
```
eg.查询平均工资大于70000的学院
```SQL
SELECT dept_name, AVG(salary)
FROM instructor
GROUP BY dept_name
HAVING AVG(salary) > 70000
```
---
- **NULL**不能用=
要用
```SQL
IS NULL
```
不是空
```SQL
IS NOT NULL
```
---
SQL 语句大体顺序：
```SQL
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
```
---
- 关于COUNT的用法
	- COUNT($*$) ：统计行数
	```SQL
	SELECT COUNT(*)
	FROM student;
	```
	- COUNT(dept_name)： 统计非空的行数
		如果某一行的dept_name是NULL，不会被算进去
	-  COUNT (DISTINCT dept_name)
	 统计有多少个不同的学院
---
- GROUP BY和HAVING的区别
	- GROUP BY是把数据按照某个字段分成一组一组
	- HAVING是对每一组的统计结果进行筛选


***
# 多表查询
- 概览：

| 类型   | 保留什么       | 本质               |
| ---- | ---------- | ---------------- |
| 笛卡尔积 | 所有可能的组合    | A的每行都和Ｂ的每行进行组合   |
| 内连接  | 只保留匹配成功的   | 两边都能对上的才要        |
| 自然连接 | 自动按同名字段内连接 | 数据库自己找同名列连接      |
| 左外连接 | 左表全部保留     | 左边一定在，右边没有就补NULL |
| 右外连接 | 右表全部保留     | 右边一定在，左边没有就补NULL |
| 全外连接 | 两表全部保留     | 两边都不丢，匹配不上补 NULL |

### 1. 笛卡尔积 - **所有可能的组合**
A表的每一行组合Ｂ表的每一行
eg.
A表

| a   | b   |
| --- | --- |
| 1   | 10  |
| 2   | 20  |
| 3   | 30  |
Ｂ表

| a   | c   |
| --- | --- |
| 1   | 100 |
| 1   | 101 |
| 2   | 200 |
| 4   | 400 |
笛卡尔积

| a   | b   | c   |
| --- | --- | --- |
| 1   | 10  | 100 |
| 1   | 10  | 101 |
| 1   | 10  | 200 |
| 1   | 10  | 400 |
| 2   | 20  | 100 |
| 2   | 20  | 101 |
| 2   | 20  | 200 |
| 2   | 20  | 400 |
| 3   | 30  | 100 |
| 3   | 30  | 101 |
| 3   | 30  | 200 |
| 3   | 30  | 400 |
```SQL
SELECT *
FROM student, takes;
```

### 2. 内连接 - 保留两边相等的行
A表

| a   | b   |
| --- | --- |
| 1   | 10  |
| 2   | 20  |
| 3   | 30  |
Ｂ表

| a   | c   |
| --- | --- |
| 1   | 100 |
| 1   | 101 |
| 2   | 200 |
| 4   | 400 |
A.a = B.a 内连接

| A.a | b   | B.a | c   |
| --- | --- | --- | --- |
| 1   | 10  | 1   | 100 |
| 1   | 10  | 1   | 101 |
| 2   | 20  | 2   | 200 |
```SQL
SELECT *
FROM A
JOIN B ON A.a = B.a;
```

###  3. 自然连接
- A和Ｂ都有同名列a
- 自然连接会自动按a相等l连接，并且a只保留一列
A表

| a   | b   |
| --- | --- |
| 1   | 10  |
| 2   | 20  |
| 3   | 30  |
Ｂ表

| a   | c   |
| --- | --- |
| 1   | 100 |
| 1   | 101 |
| 2   | 200 |
| 4   | 400 |
自然连接

| a   | b   | c   |
| --- | --- | --- |
| 1   | 10  | 100 |
| 1   | 10  | 101 |
| 2   | 20  | 200 |
```SQL
SELECT *
FROM A NATURAL JOIN B;
```
等价于
```SQL
SELECT A.a, A.b, B.c
FROM A
JOIN B ON A.a = B.a;
```
注意
*自然连接会用左右的同名列作为连接条件！*
假设A表有a b x，B表有a b y
那么
```SQL
SELECT *
FROM A NATURAL JOIN B
```
等价于
```SQL
SELECT *
FROM A 
JOIN B ON A.a = B.a
AND
A.b = B.b
```
### 4. 左外连接

| A.a | b   | B.a  | c    |
| --- | --- | ---- | ---- |
| 1   | 10  | 1    | 100  |
| 1   | 10  | 1    | 101  |
| 2   | 20  | 2    | 200  |
| 3   | 30  | NULL | NULL |
```SQL
SELECT *
FROM A
LEFT OUTER JOIN B ON A.a = B.a;
```
### 5. 右外连接

| A.a  | b    | B.a | c   |
| ---- | ---- | --- | --- |
| 1    | 10   | 1   | 100 |
| 1    | 10   | 1   | 101 |
| 2    | 20   | 2   | 200 |
| NULL | NULL | 4   | 400 |
```SQL
SELECT *
FROM A
RIGHT OUTER JOIN B ON A.a = B.a;
```
### 6. 全外连接

| A.a  | b    | B.a  | c    |
| ---- | ---- | ---- | ---- |
| 1    | 10   | 1    | 100  |
| 1    | 10   | 1    | 101  |
| 2    | 20   | 2    | 200  |
| 3    | 30   | NULL | NULL |
| NULL | NULL | 4    | 400  |
