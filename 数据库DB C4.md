# P1. 视图

^b89568

#### 视图是什么？
视图不是一张真正存数据的表，而是：
```
基于查询结果d定义出来的“虚表”
```
#### 为什么需要视图？
1. **简化查询**
	如果一个查询很复杂，可以先定义成视图
	以后直接查视图，不用每次写复杂的SQL
2. **隐藏细节**
	比如学生只需要看到
```
	姓名、课程、成绩
```
	不应该看到：
```
	身份证号、工资、内部编号
```
	可以给学生一个视图，只暴露部分列
3. 提高安全性
	不同用户看到不同视图
#### 视图怎么定义？
基本格式：
```SQL
CREATE VIEW 视图名 AS
SELECT ...
FROM ...
WHERE ...;
```
#### 其他有关考点
- 视图是由查询定义的虚表，通常不实际存储数据
- 视图依赖j基本表，如果底层表student的数据变了，那么视图查出来的结果也会跟着变
- 删除视图不等于删除表
- 视图能不能更新？一般不更新，简单的有时候可以更新，但复杂视图一般不能更新
# P2. 完整性约束

^3fd0ca

- 数据库为了保证数据正确、合理、一致而设置的规则
他不是查询语句，而是建表时规定：
##### `哪些数据能进来，哪些数据不能进来`

#### 1. NOT NULL: 非空约束
- 表示这一列不能没有值
```SQL
CREATE TABLE student(
	ID varchar(5) NOT NULL,
	name varchar(20) NOT NULL,
	dept_name varchar(20),
	tot_cred numeric(3,0)
);
```
#### 2. UNIQUE: 唯一约束
- 表示这一列或这一组列不能重复
```SQL
CREATE TABLE instructor(
	ID varchar(5),
	name varchar(20),
	email varchar(50) UNIQUE
);
```
#### 3. PRIMERY KEY：主键约束
- 主键 = 唯一 + 非空
```SQL
CREATE TABLE student(
	ID varchar(5),
	name varchar(20),
	dept_name varchar(20)
	year numeric(3,0),
	PRIMARY KEY(ID)
);
```
#### 4. FOREIGN KEY: 外键约束
- 外键用来保证两个表之间的引用关系正确
- 比如
```
takes.ID 应该来自 student.ID
```
```SQL
CREATE TABLE takes(
	ID varchar(5),
	course_id varchar(8),
	sec_id varchar(8),
	semester varchar(6),
	year numeric (4,0),
	grade varchar(2),
	PRIMARY KEY (ID, course_id, sec_id, semester,year),
	FOREIGN KEY(ID) REFERENCES student(ID)
);
```
#### 5. CHECK: 条件约束
- CHECK用来限制某一列的取值范围
```SQL
CREATE TABLE student(
	ID varchar(5),
	name varchar(20),
	tot_cred numeric(3,0),
	CHECK(tot_cred >= 0)
);
```
#### 6. DEFAULT: 默认值
- 如果插入数据时没有给这一列赋值，就自动使用默认值。
```SQL
CREATE TABLE student(
	ID varchar(5),
	name varchar(20),
	tot_cred numeric(3.0) DEFAULT 0
);
```
---
#### 三类完整性的对应关系

| 理论概念     | SQL中常见表现                     |
| -------- | ---------------------------- |
| 实体完整性    | PRIMARY KEY，NOT NULL, UNIQUE |
| 参照完整性    | FOREIGN KEY                  |
| 用户自定义完整性 | CHECK，DEFAULT，NOT NULL       |

---
### 外键违反时会发生什么
#### 插入失败
#### 删除被引用数据的问题
#### 外键的级联操作 ON DELETE
