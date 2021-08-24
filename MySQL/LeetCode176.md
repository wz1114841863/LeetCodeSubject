### 题目：

编写一个 SQL 查询，获取 `Employee` 表中第二高的薪水（Salary） 。

### 难度：

简单。

### 示例：

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

例如上述 Employee 表，SQL查询应该返回 200 作为第二高的薪水。如果不存在第二高的薪水，那么查询应返回 null。

```
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```

### 解题思路：

```
    按照薪水 distinct + order desc 排序即可。使用limit 1，1；
    关键在于输出null, 所以需要套两层select。
```

### 代码实现：

```c++
select
    (select 
        distinct Salary 
    from 
        Employee
    order by Salary desc
    limit 1, 1 )
as  SecondHighestSalary;
```

### 知识点：

还可以使用函数来解决问题。

### 官方代码：

```
SELECT
    IFNULL(
      (SELECT DISTINCT Salary
       FROM Employee
       ORDER BY Salary DESC
        LIMIT 1 OFFSET 1),
    NULL) AS SecondHighestSalary
```

### 热评：

```
select null
//返回null。
```

