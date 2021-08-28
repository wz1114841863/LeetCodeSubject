### 题目：

```
Find the largest country (by area) in each continent, show the continent, the name and the area:
```

### 解题思路：

```
利用自联结或者是分组。
```

### 代码实现：

```mysql
/*
# 利用分组和聚集函数
select continent, name,  area
from world
where area in (
          SELECT max(area) as area
          FROM world 
          group by continent
)
order by name
;
*/
#利用
select continent, name , area
from world x
where area >= all(
      select area
      from world y
      where x.continent = y.continent and area > 0
)
;
```

### 知识点

```
利用自联结或者子查询可以实现分组的效果。
在关联子查询中，对于外部查询返回的每一行数据，内部查询都要执行一次。另外，在关联子查询中是信息流是双向的。外部查询的每行数据传递一个值给子查询，然后子查询为每一行数据执行一次并返回它的记录。然后，外部查询根据返回的记录做出决策。
```

