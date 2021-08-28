### 题目：

```
Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)
```

### 解题思路：

```
关键字All的使用和NULL的排除。
```

### 代码实现：

```mysql
select name
from world
where gdp > All(
          select gdp
          from world
          where continent = 'Europe' and   gdp>0 
) 
;
```

### 知识点

```
ALL可以用于排查一整个列表。
```

