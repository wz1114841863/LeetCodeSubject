### 题目：

假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**注意：**给定 *n* 是一个正整数。

### 难度：

简单。

### 示例：

```
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

### 解题思路：

```
到达第n阶梯的方法有 (第n-1阶梯 ) + (第 n-2 阶梯 )
```

### 代码实现：

```c++
class Solution {
public:
    int climbStairs(int n) {
        //排除特殊情况
        if( n <= 2){
            return n;
        }
        //存储n-1， n-2；
        long step1 = 1, step2 = 2, temp;
        //一直计算到第n阶：
        for(int i=3; i<=n; i++){
            temp = step1  + step2 ;
            step1 = step2;
            step2 = temp;
        }
        return step2;
    }
};
```

### 知识点：

数学对于程序员的重要性。

### 官方代码：

```
class Solution {
public:
    vector<vector<long long>> multiply(vector<vector<long long>> &a, vector<vector<long long>> &b) {
        vector<vector<long long>> c(2, vector<long long>(2));
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < 2; j++) {
                c[i][j] = a[i][0] * b[0][j] + a[i][1] * b[1][j];
            }
        }
        return c;
    }

    vector<vector<long long>> matrixPow(vector<vector<long long>> a, int n) {
        vector<vector<long long>> ret = {{1, 0}, {0, 1}};
        while (n > 0) {
            if ((n & 1) == 1) {
                ret = multiply(ret, a);
            }
            n >>= 1;
            a = multiply(a, a);
        }
        return ret;
    }

    int climbStairs(int n) {
        vector<vector<long long>> ret = {{1, 1}, {1, 0}};
        vector<vector<long long>> res = matrixPow(ret, n);
        return res[0][0];
    }
};
```

### 热评：

```
看完官方题解，我发现，像这么经典的动态规划题你再用动态规划解的话就弱了，一定要秀一把自己的数学肌肉才行。建议这题可以加上“数学”的标签了。没有学好线性代数的我留下了没有数学的眼泪。
```

