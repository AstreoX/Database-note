# 形式化关系查询语言
- 本质上就是不用SQL，而是用数学符号来写查询
```SQL
SELECT name
FROM student
WHERE dept_name = ‘Comp. Sci.’;
```
关系代数
```关系代数
π_name(σ_dept_name='Comp. Sci.'(student))
```
### 核心的符号

| 运算   | 符号        | 对应SQL     | 作用          |
| ---- | --------- | --------- | ----------- |
| 选择   | $\sigma$  | WHERE     | 选行          |
| 投影   | $\pi$     | SELECT    | 选列          |
| 并    | $\cup$    | UNION     | 并集          |
| 差    | $-$       | EXCEPT    | 差集（前者有后者没有） |
| 交    | $\cap$    | INTERSECT | 交集          |
| 笛卡尔积 | $\times$  | FROM A, B | 所有组合        |
| 连接   | $\bowtie$ | JOIN      | 按条件拼表       |
| 重命名  | $\rho$    | AS        | 别名          |
### 关系代数写题套路
1. 要从哪张表查？确定关系
2. 有什么条件？用$\sigma$
3. 最后要显示什么？用$\pi$
### 常用模板
#### 单表条件查询
$\pi$  \_列($\sigma$\_条件(R))
```SQL
SELECT 列
FROM R
WHERE 条件;
```
eg.

$\pi$\_name($\sigma$\_age>20(student))

等价于SQL
```SQL
SELECT name
FROM student
WHERE age>20;
```

#### 双表连接查询
$\pi$ \_列(R $\bowtie$\_连接条件 S)
```SQL
SELECT 列
FROM R
JOIN S
ON 连接条件;
```
eg.

$\pi$\_name,course_id(student $\bowtie$ (student.ID=takes.ID) takes)

等价于
```SQL
SELECT name,course_id
FROM student
JOIN takes
ON student.ID=takes.ID;
```
或者：
$\pi$\_列( R $\bowtie$\_连接条件(R $\times$ S))
```SQL
SELECT 列
FROM R,S
WHERE 连接条件
```
eg.

$\pi$\_name,course_id($\sigma$\_student.ID=takes.ID(student $\times$ takes))

```SQL
SELECT name,course_id
FROM student,takes
WHERE student.ID=takes.ID;
```
#### 多条件查询
$\pi$\_列($\sigma$\_条件 1 $\land$ 条件2)
```SQL
SELECT 列
FROM R
WHERE 条件1
AND 条件2;
```
$\lor$ -> OR 
#### 并集（UNION）
R $\cup$ S
```SQL
SELECT ...
FROM R

UNION

SELECT ...
FROM S;
```

#### 交集(INTERSECT)
R $\cap$ S
```SQL
SELECT ...
FROM R

INTERSECT

SELECT ...
FROM S;
```

#### 差集
R $-$ S
```SQL
SELECT ...
FROM R

EXCEPT

SELECT ...
FROM S;
```