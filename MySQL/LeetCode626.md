### 题目：

小美是一所中学的信息科技老师，她有一张 seat 座位表，平时用来储存学生名字和与他们相对应的座位 id。

其中纵列的 id 是连续递增的

小美想改变相邻俩学生的座位。

你能不能帮她写一个 SQL query 来输出小美想要的结果呢？

### 难度：

中等。

### 示例：

```
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |
+---------+---------+
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |
+---------+---------+
```

### 解题思路：

```c++
利用case then 语句。
```

### 代码实现：

```c++
Select (
    case
        when mod(id, 2) = 1 and id = (select max(id) from seat ) then id
        when mod(id, 2) = 1 then id + 1
        else id - 1
    end) as id, student
from seat 
order by id
;
```

### 知识点：

数据库的题解法并不唯一。

### 官方代码：

```c++
SELECT
    s1.id, COALESCE(s2.student, s1.student) AS student
FROM
    seat s1
        LEFT JOIN
    seat s2 ON ((s1.id + 1) ^ 1) - 1 = s2.id
ORDER BY s1.id;
```

### 热评：

```
select rank() over(order by (id-1)^1) as id,student from seat
```

