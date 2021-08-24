### 题目：

表1: Person

```
+-------------+---------+
| 列名         | 类型     |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
PersonId 是上表主键
```


表2: Address

```
+-------------+---------+
| 列名         | 类型    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId 是上表主键
```


编写一个 SQL 查询，满足条件：无论 person 是否有地址信息，都需要基于上述两表提供 person 的以下信息：

```
 FirstName, LastName, City, State
```



### 难度：

简单。

### 示例：

```
输入：
{"headers": {"Person": ["PersonId", "LastName", "FirstName"], "Address": ["AddressId", "PersonId", "City", "State"]}, "rows": {"Person": [[1, "Wang", "Allen"]], "Address": [[1, 2, "New York City", "New York"]]}}
输出：
{"headers": ["FirstName", "LastName", "City", "State"], "values": [["Allen", "Wang", null, null]]}
```

### 解题思路：

```
    关键在于：无论 person 是否有地址信息
    所以使用了左联结。
```

### 代码实现：

```c++
# Write your MySQL query statement below
select
    P.FirstName, P.LastName, A.city, A.state
from 
    Person as P left outer join Address as A
on 
    P.PersonId = A.PersonId;
```

### 知识点：

区分inner join 和 left join。

### 官方代码：

```
select FirstName, LastName, City, State
from Person left join Address
on Person.PersonId = Address.PersonId
;
```

### 热评：

```
DROP DATABASE test;
```

