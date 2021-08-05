### 题目：

实现 [pow(*x*, *n*)](https://www.cplusplus.com/reference/valarray/pow/) ，即计算 x 的 n 次幂函数（即 x^n）。

### 难度：

中等。

### 示例：

```
输入：x = 2.00000, n = 10
输出：1024.00000
```

### 解题思路：

```
如果 n 小于 0 ，记录 ， 取绝对值。之后再取分数。
递归版本可以通过。
迭代版本通过所有的示例后依旧显示超出时间限制
```

### 代码实现：

```c++
class Solution {
public:
    double myPow(double x, long long n) {
        if (n == 0)
            return 1;
        if (n < 0)
            return 1 / myPow(x, -n);
        double mid = myPow(x, n / 2);
        if (n & 1)
            return mid * mid * x;
        return mid * mid;
    }
};
/*
class Solution {
public:
    double myPow(double x, long long n) {
        //标志位
        int flag = 1;
        if( n == 0 || x == double(1)){
            return 1;
        }else if( n == 1 ){
            return x;
        }else if( n == -1){
            return 1/x;
        }else if( n < 0){
            flag = -1;
            n = -n;
        }
        //返回值
        double res = x;
        //计数
        double count = 1;
        for(count = 2; count <= n; ){
            count = 2 * count;
            res = res * res;
            //cout << "res:  " << res << endl;
        }
        //cout << "cout:  " << count << endl;
        for(count = count / 2 + 1; count <= n; count++){
            res = res * x;
        }
        if(flag == -1){
            return  1 / res;
        }
        return  res;
    }
};
*/
```

### 知识点：

迭代版本的不知道哪有问题。通过了所有了测试用例。最终显示超时。头大。

观看了官方代码的迭代版本。只有一句，”对不起，我是废物。“

### 官方代码：

```
class Solution {
public:
    double quickMul(double x, long long N) {
        double ans = 1.0;
        // 贡献的初始值为 x
        double x_contribute = x;
        // 在对 N 进行二进制拆分的同时计算答案
        while (N > 0) {
            if (N % 2 == 1) {
                // 如果 N 二进制表示的最低位为 1，那么需要计入贡献
                ans *= x_contribute;
            }
            // 将贡献不断地平方
            x_contribute *= x_contribute;
            // 舍弃 N 二进制表示的最低位，这样我们每次只要判断最低位即可
            N /= 2;
        }
        return ans;
    }

    double myPow(double x, int n) {
        long long N = n;
        return N >= 0 ? quickMul(x, N) : 1.0 / quickMul(x, -N);
    }
};

```

### 热评：

```
方法二真是妙蛙种子吃着妙脆角妙进了米奇妙妙屋，妙到家了。
```

```
//啪的一下就击败100了 很快啊
class Solution {
public:
    double myPow(double x, int n) {
        return pow(x,n);
    }
};
```

