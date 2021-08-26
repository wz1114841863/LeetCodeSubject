### 题目：

```
--表weather 
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
id 是这个表的主键
该表包含特定日期的温度信息
```

编写一个 SQL 查询，来查找与之前（昨天的）日期相比温度更高的所有日期的 `id` 。

返回结果 **不要求顺序** 。

### 难度：

简单。

### 示例：

```
--查询结果格式如下例
Result table:
+----+
| id |
+----+
| 2  |
| 4  |
+----+
2015-01-02 的温度比前一天高（10 -> 25）
2015-01-04 的温度比前一天高（20 -> 30）
```

### 解题思路：

```
使用自联结 + 两个判断条件。
```

### 代码实现：

```c++
# Write your MySQL query statement below
select c1.id
from Weather as c1, Weather as c2
where c1.temperature > c2.temperature and dateDiff(c1.RecordDate,c2.RecordDate) = 1
;
```

### 知识点：

初始代码：

--错误代码

--Write your MySQL query statement below

select c1.id
from Weather as c1, Weather as c2
where c1.temperature > c2.temperature and c1.RecordDate = c2.RecordDate + 1
;

原因：

```
--时间格式数据库不支持直接用加减呗，数学中2021-6-30加1等于2021-7-01吗，改成datediff就行。
```

### 官方代码：

```
SELECT
    weather.id AS 'Id'
FROM
    weather
        JOIN
    weather w ON DATEDIFF(weather.date, w.date) = 1
        AND weather.Temperature > w.Temperature
；
```

### 热评：

```
学到了DATEDIFF是两个日期的天数差集
```

