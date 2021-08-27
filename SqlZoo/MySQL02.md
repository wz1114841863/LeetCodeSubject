### 题目：

```
The expression subject IN ('Chemistry','Physics') can be used as a value - it will be 0 or 1.

Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.
```

### 解题思路：

```
关键在于如何实现  list Chemistry and Physics last. 
```

### 代码实现：

```mysql
SELECT winner, subject
FROM nobel
WHERE yr=1984
ORDER BY subject IN ('Physics','Chemistry'), subject,winner
;
```

### 知识点

```
观察后可知，subject IN ('Chemistry','Physics')返回的是0和1，所以先按照0 和 1 排序，自然就会将 Chemistry 和 Physics 排在最后面。
```

具体图片见  /Image/MySQL01.png。

