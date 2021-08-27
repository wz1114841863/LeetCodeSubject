### 题目：

编写一个 SQL 查询，来删除 `Person` 表中所有重复的电子邮箱，重复的邮箱里只保留 **Id** *最小* 的那个。

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
Id 是这个表的主键。
```

### 难度：

简单

### 示例：

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+。
```

### 解题思路：

```
错误想法：
查找distinct 邮箱，根据id 排序
返回Id
删除不在这些id中的其他id

改进：
    根据Email分组
    返回每组中最小的id
    删除其余id
```

### 代码实现：

```c++
# Write your MySQL query statement below
delete from Person
where id not in(
        select a.id from(
            select  min(id) as id, Email 
            from Person
            group by Email
        ) as a 
)
;
```

### 知识点：

```
distinct id，email
```

由于id是主键，所以返回的列都算不一样的。

### 官方代码：

```
官方代码：
delete p1 from Person p1 ,Person p2
where p1.Email =p2.Email and p1.Id > p2.Id 
;
```

### 热评：

```
这道题有毒.. 无论怎么查询 都是输出3条数据 , 即使像下面这样指定了Id , 依然会返回三条数据:\n SELECT * FROM Person WHERE Id = 1 ;
```

