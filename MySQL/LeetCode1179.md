### 题目：

部门表 `Department`：

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| revenue       | int     |
| month         | varchar |
+---------------+---------+
(id, month) 是表的联合主键。
这个表格有关于每个部门每月收入的信息。
月份（month）可以取下列值 ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"]。
```

### 难度：

简单。

### 示例：

```
编写一个 SQL 查询来重新格式化表，使得新的表中有一个部门 id 列和一些对应 每个月 的收入（revenue）列。

查询结果格式如下面的示例所示：

Department 表：
+------+---------+-------+
| id   | revenue | month |
+------+---------+-------+
| 1    | 8000    | Jan   |
| 2    | 9000    | Jan   |
| 3    | 10000   | Feb   |
| 1    | 7000    | Feb   |
| 1    | 6000    | Mar   |
+------+---------+-------+

查询得到的结果表：
+------+-------------+-------------+-------------+-----+-------------+
| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
+------+-------------+-------------+-------------+-----+-------------+
| 1    | 8000        | 7000        | 6000        | ... | null        |
| 2    | 9000        | null        | null        | ... | null        |
| 3    | null        | 10000       | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
```

### 解题思路：

```c++
利用group by 进行分组。
利用case then 实现行列转换。
```

### 代码实现：

```c++
/*
-- 错误解法：
    对group id 的理解不到位。
select id,
    (case month when 'Jan' then revenue else null end) as Jan_Revenue,
    (case month when 'Feb' then revenue else null end) as Feb_Revenue,
    (case month when 'Mar' then revenue else null end) as Mar_Revenue,
    (case month when 'Apr' then revenue else null end) as Apr_Revenue,
    (case month when 'May' then revenue else null end) as May_Revenue,
    (case month when 'Jun' then revenue else null end) as Jun_Revenue,
    (case month when 'Jul' then revenue else null end) as Jul_Revenue,
    (case month when 'Aug' then revenue else null end) as Aug_Revenue,
    (case month when 'Sep' then revenue else null end) as Sep_Revenue,
    (case month when 'Oct' then revenue else null end) as Oct_Revenue,
    (case month when 'Nov' then revenue else null end) as Nov_Revenue,
    (case month when 'Dec' then revenue else null end) as Dec_Revenue
from Department
group by id
*/
select id
    , sum(case `month` when 'Jan' then revenue else null end) as Jan_Revenue
    , sum(case `month` when 'Feb' then revenue else null end) as Feb_Revenue
    , sum(case `month` when 'Mar' then revenue else null end) as Mar_Revenue
    , sum(case `month` when 'Apr' then revenue else null end) as Apr_Revenue
    , sum(case `month` when 'May' then revenue else null end) as May_Revenue
    , sum(case `month` when 'Jun' then revenue else null end) as Jun_Revenue
    , sum(case `month` when 'Jul' then revenue else null end) as Jul_Revenue
    , sum(case `month` when 'Aug' then revenue else null end) as Aug_Revenue
    , sum(case `month` when 'Sep' then revenue else null end) as Sep_Revenue
    , sum(case `month` when 'Oct' then revenue else null end) as Oct_Revenue
    , sum(case `month` when 'Nov' then revenue else null end) as Nov_Revenue
    , sum(case `month` when 'Dec' then revenue else null end) as Dec_Revenue
from Department
group by id
;
```

### 知识点：

对group by id进一步理解。

为什么这里必须需要一个聚合函数？

因为group by id 后仅会显示id相同的查询出来的最上方的一条语句。

所以如果想要合并查询出来的所有结果，就需要使用聚合函数。

```
select id,
    max(case month when 'Jan' then revenue else null end) as Jan_Revenue,
    max(case month when 'Feb' then revenue else null end) as Feb_Revenue,
    max(case month when 'Mar' then revenue else null end) as Mar_Revenue,
    max(case month when 'Apr' then revenue else null end) as Apr_Revenue,
    max(case month when 'May' then revenue else null end) as May_Revenue,
    max(case month when 'Jun' then revenue else null end) as Jun_Revenue,
    max(case month when 'Jul' then revenue else null end) as Jul_Revenue,
    max(case month when 'Aug' then revenue else null end) as Aug_Revenue,
    max(case month when 'Sep' then revenue else null end) as Sep_Revenue,
    max(case month when 'Oct' then revenue else null end) as Oct_Revenue,
    max(case month when 'Nov' then revenue else null end) as Nov_Revenue,
    max(case month when 'Dec' then revenue else null end) as Dec_Revenue
from Department
group by id
;
```

同样运行通过，所以这里的具体的聚合函数不重要。

### 官方代码：

```c++
select id,
    sum(case month when 'Jan' then revenue end) as Jan_Revenue,
    sum(case month when 'Feb' then revenue end) as Feb_Revenue,
    sum(case month when 'Mar' then revenue end) as Mar_Revenue,
    sum(case month when 'Apr' then revenue end) as Apr_Revenue,
    sum(case month when 'May' then revenue end) as May_Revenue,
    sum(case month when 'Jun' then revenue end) as Jun_Revenue,
    sum(case month when 'Jul' then revenue end) as Jul_Revenue,
    sum(case month when 'Aug' then revenue end) as Aug_Revenue,
    sum(case month when 'Sep' then revenue end) as Sep_Revenue,
    sum(case month when 'Oct' then revenue end) as Oct_Revenue,
    sum(case month when 'Nov' then revenue end) as Nov_Revenue,
    sum(case month when 'Dec' then revenue end) as Dec_Revenue
from Department
group by id
```

### 热评：

```
每个题都不会做，气死了！
```

