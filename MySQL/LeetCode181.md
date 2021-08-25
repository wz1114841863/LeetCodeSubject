### 题目：

`Employee` 表包含所有员工，他们的经理也属于员工。每个员工都有一个 Id，此外还有一列对应员工的经理的 Id。

```
+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+
```

给定 `Employee` 表，编写一个 SQL 查询，该查询可以获取收入超过他们经理的员工的姓名。

### 难度：

简单。

### 示例：

在上面的表格中，Joe 是唯一一个收入超过他的经理的员工。

```
+----------+
| Employee |
+----------+
| Joe      |
+----------+
```

### 解题思路：

```
查询经理工资。
查询超过经理工资的员工。
使用内联结或自查询。
```

### 代码实现：

```c++
select 
    e1.Name as Employee
from 
    Employee as e1, Employee as e2
where
        e1.ManagerId = e2.Id
    and
        e1.Salary > e2.Salary
    ;
```

### 知识点：

内联结用where和 join ... on 没区别。

还有一种子查询的形式。

### 官方代码：

```
SELECT
     a.NAME AS Employee
FROM Employee AS a JOIN Employee AS b
     ON a.ManagerId = b.Id
     AND a.Salary > b.Salary
;
```

### 热评：

```
收入超过经理，我还当经理干嘛。
```

