### 题目：

某城市开了一家新的电影院，吸引了很多人过来看电影。该电影院特别注意用户体验，专门有个 LED显示板做电影推荐，上面公布着影评和相关电影描述。

作为该电影院的信息部主管，您需要编写一个 SQL查询，找出所有影片描述为非 boring (不无聊) 的并且 id 为奇数 的影片，结果请按等级 rating 排列。

```
+---------+-----------+--------------+-----------+
|   id    | movie     |  description |  rating   |
+---------+-----------+--------------+-----------+
|   1     | War       |   great 3D   |   8.9     |
|   2     | Science   |   fiction    |   8.5     |
|   3     | irish     |   boring     |   6.2     |
|   4     | Ice song  |   Fantacy    |   8.6     |
|   5     | House card|   Interesting|   9.1     |
+---------+-----------+--------------+-----------+
```

### 难度：

简单。

### 示例：

```
+---------+-----------+--------------+-----------+
|   id    | movie     |  description |  rating   |
+---------+-----------+--------------+-----------+
|   5     | House card|   Interesting|   9.1     |
|   1     | War       |   great 3D   |   8.9     |
+---------+-----------+--------------+-----------+
```

### 解题思路：

```c++
使用where语句实现条件查找。
```

### 代码实现：

```c++
select id, movie, description, rating
from cinema
where description <> 'boring' and id & 1
--    and mod(id, 2) = 1
--    and id % 2 <> 0
order by rating desc
```

### 知识点：

用 位运算 或者 自带函数更快。

### 官方代码：

```c++
select *
from cinema
where mod(id, 2) = 1 and description != 'boring'
order by rating DESC
;
```

### 热评：

```
我是主管还用我写SQL?
```

