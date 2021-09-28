### 题目：

有一个`courses` 表 ，有: **student (学生)** 和 **class (课程)**。

请列出所有超过或等于5名学生的课。

例如，表：

```
+---------+------------+
| student | class      |
+---------+------------+
| A       | Math       |
| B       | English    |
| C       | Math       |
| D       | Biology    |
| E       | Math       |
| F       | Computer   |
| G       | Math       |
| H       | Math       |
| I       | Math       |
+---------+------------+

```

### 难度：

简单。

### 示例：

```
+---------+
| class   |
+---------+
| Math    |
+---------+
```

### 解题思路：

```c++
用distinct语句进行筛选。
```

### 代码实现：

```c++
select class
from courses
group by class
having count(distinct student) >= 5
;
```

### 知识点：

注意重修。

### 官方代码：

```c++
SELECT
    class
FROM
    (SELECT
        class, COUNT(DISTINCT student) AS num
    FROM
        courses
    GROUP BY class) AS temp_table
WHERE
    num >= 5
;
```

### 热评：

```
一门课能被同一个学生选好几次就离谱，终于知道抢课为什么抢不到了，妈的
```

