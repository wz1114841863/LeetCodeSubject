### 题目：

实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

### 难度：

简单。

### 示例：

```
输入: 8
输出: 2
说明: 8 的平方根是 2.82842..., 
     由于返回类型是整数，小数部分将被舍去。
```

### 解题思路：

```
利用二分法找到一个数: num
    num*num <= x 且 （num + 1）（num+1) > x;
```

### 代码实现：

```c++
class Solution {
public:
    int mySqrt(int x) {
        //处理特殊情况
        if( x == 0 || x == 1){
            return x;
        }
        //利用二分法查找
        int left = 0;
        int right = x - 1;
        int middle;
        while(left <= right){
            middle = (left + right) / 2;
            if( middle == 0){
                left = middle + 1;
            }else if( middle == x / middle){
                return middle;
            }else if( middle < x / middle){
                left = middle + 1;
            }else if( middle > x / middle){
                right = middle - 1;
            }   
        }
        return left - 1;
    }
};
```

### 知识点：

我使用的是二分法，最快的应该是牛顿迭代法。没想到。

### 官方代码：

```
class Solution {
public:
    int mySqrt(int x) {
        if (x == 0) {
            return 0;
        }

        double C = x, x0 = x;
        while (true) {
            double xi = 0.5 * (x0 + C / x0);
            if (fabs(x0 - xi) < 1e-7) {
                break;
            }
            x0 = xi;
        }
        return int(x0);
    }
};

```

### 热评：

```
牛顿，永远滴神。
```

