### 题目：

```
Two ways to be big: A country is big if it has an area of more than 3 million sq km or it has a population of more than 250 million.
Show the countries that are big by area or big by population. Show name,population and area.
```

### 解题思路：

```
使用条件OR来满足不同条件。
需要注意的点：
	distinct 的使用来避免重复结果。
```

### 代码实现：

```mysql
select distinct name, population, area
from world
where area > 3000000 or population > 250000000
;
```

### 知识点

```
or 与 UNION 一般可以替换。
select distinct name, population, area
from world
where area > 3000000
UNION
select distinct name, population, area
from world
where population > 250000000
;
```

