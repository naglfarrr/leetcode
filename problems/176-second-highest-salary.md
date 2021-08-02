# 题目地址 (176. 第二高的工资)

https://leetcode.com/problems/second-highest-salary/

## 题目描述

从 `Employee` 表中查询第二高的工资，命名为 `SecondHighestSalary`。
如果不存在第二高的工资，返回 `null`。

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

## 思路

### 方法一

- 使用 `ORDER BY ... DESC` 将结果降序排序；
- `LIMIT offset count` 可以从结果中截取一部分连续的行；
  - 前一个参数 `offset` 表示截取起点的偏移量。 第一行的偏移量是 0，所以第二高的工资在“1”行；
  - 后一个参数 `count` 表示截取几行，本题只取一行。
- 可能存在并列的最高工资，所以使用 `DISTINCT` 关键字，只选取不同的值；
- 如果结果为空，使用 `IFNULL()` 将其替换成 `null`。

### 方法二

`MAX()` 函数可以查询出最大值，对结果再次查询就是第二大的。

## 代码

### 方法一

```sql
SELECT IFNULL((SELECT DISTINCT Salary
              FROM Employee
              ORDER BY Salary DESC
              LIMIT 1,1), null) AS SecondHighestSalary;
```

### 方法二

```sql
SELECT MAX(Salary) AS SecondHighestSalary
FROM Employee 
WHERE Salary < (SELECT MAX(Salary)
                FROM Employee);
```

