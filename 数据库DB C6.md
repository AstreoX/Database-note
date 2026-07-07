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