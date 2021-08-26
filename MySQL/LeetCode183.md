### 题目：

某网站包含两个表，`Customers` 表和 `Orders` 表。编写一个 SQL 查询，找出所有从不订购任何东西的客户。

```
--Customers表
+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+
```

```
--Orders表
+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+
```

### 难度：

简单

### 示例：

```
+-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+
```

### 解题思路：

```
查询orders表中不存在的CustomerId
```

### 代码实现：

```c++
# Write your MySQL query statement below

 select c.name as Customers
 from Customers as c left join orders as o
 on C.id = o.CustomerId
 where  o.id is null;

```

### 知识点：

Sql的代码一般不唯一。

### 官方代码：

```
 select customers.name as 'Customers'
from customers
where customers.id not in
(
    select customerid from orders
);
```

### 热评：

```
我使用了左连接查询，学到了只能采用IS NULL或IS NOT NULL，而不能采用=, <, <>, !=这些操作符来判断NULL。
```

