### 题目：

给定一个 正整数 num ，编写一个函数，如果 num 是一个完全平方数，则返回 true ，否则返回 false 。

进阶：不要 使用任何内置的库函数，如  sqrt 。

### 难度：

简单。

### 示例：

```
输入：num = 16
输出：true
```

### 解题思路：

```c++
使用二分法：
需要注意的点：
    1.越界、边界情况。
    2.浮点数转为整数时精度损失。
    3.python由于数据长度不限所以不用考虑这些。
```

### 代码实现：

```c++
class Solution {
public:
    bool isPerfectSquare(int num) {
        int left = 1;
        int right = num;  // 防止溢出 INT_MAX
        while (left <= right) {
            int middle = left + ((right - left) >> 1);
            // 需要考虑越界问题
            int tmp = num / middle;
            if (tmp == middle) {
                if (num % middle == 0)  // 避免了使用long long类型。
                    return true;
                left = middle + 1;
            } else if (tmp < middle) {
                right = middle - 1;
            } else {
                left = middle + 1;
            }
        }
        return false;
    }
};
```

```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        left = 1
        right = num + 1
        while left < right:
            mid = (left + right) // 2
            if mid * mid == num:
                return True
            elif mid * mid < num :
                left = mid + 1
            elif mid * mid > num:
                right = mid

        return False
```

### 知识点：

python与C++的语法区别可以由以上得到很好的体现。

### 官方代码：

```c++
利用 1+3+5+7+9+…+(2n-1)=n^2，即完全平方数肯定是前n个连续奇数的和class Solution {
public:
    bool isPerfectSquare(int num) {
        int left = 0, right = num;
        while (left <= right) {
            int mid = (right - left) / 2 + left;
            long square = (long) mid * mid;
            if (square < num) {
                left = mid + 1;
            } else if (square > num) {
                right = mid - 1;
            } else {
                return true;
            }
        }
        return false;
    }
};
```

### 热评：

```
利用 1+3+5+7+9+…+(2n-1)=n^2，即完全平方数肯定是前n个连续奇数的和。
时间复杂度考虑： X = n^2 = 1 + 3 + 5 + ... + (2n - 1).所以是复杂度是根号x.
```

