### 题目：

编写一个 SQL 查询，查找 `Person` 表中所有重复的电子邮箱。

### 难度：

简单。

### 示例：

```
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
--查找结果。
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
```

### 解题思路：

```
按email分类，返回分类后数目大于1的id。
```

### 代码实现：

```c++
select email
from person 
group by email
having count(email) > 1
;
```

### 知识点：

对于语句的执行顺序还不是很清楚。

### 官方代码：

```
select Email from
(
  select Email, count(Email) as num
  from Person
  group by Email
) as statistic
where num > 1
;
```

### 热评：

```
select distinct(p1.Email) from Person p1  
join Person  p2 on p1.Email = p2.Email AND p1.Id <> p2.Id
```

