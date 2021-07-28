### 题目：

给定两个整数，被除数 dividend 和除数 divisor。将两数相除，要求不使用乘法、除法和 mod 运算符。

返回被除数 dividend 除以除数 divisor 得到的商。

整数除法的结果应当截去（truncate）其小数部分，例如：truncate(8.345) = 8 以及 truncate(-2.7335) = -2

### 难度：

中等。

### 示例：

```
输入: dividend = 10, divisor = 3
输出: 3
解释: 10/3 = truncate(3.33333..) = truncate(3) = 3
```

### 解题思路：

```
初始想法：
如果不用除号，自然要用位操作符来实现除法。
先用2^n实现逼近除数。最后用减法得到最终结果。
但是要考虑很多细节，比如说符号正负、结果溢出等。

改进：
利用递归和二分法。
```

### 代码实现：

```c++
class Solution {
public:
    //利用递归来获取结果
    int div(int a, int b){
        //递归终止条件。
        if( a > b){
            return 0;
        }
        //计数。
        int res = 1;
        //记录b值
        int step = b;
        //循环遍历，判断a 是否大于b的 n 倍
        while(a - step <= step ){  //与 a <= step + step  比较、
            step += step;
            res += res;
        }
        return res + div(a - step, b);
    }
    int divide(int dividend, int divisor) {
            //判断被除数是否为零。如果是零直接返回零。
            if( dividend == 0){
                return 0;
            }
            //如果除数是1，直接返回。
            if( divisor == 1){
                return dividend;
            }
            //可能溢出的结果
            if(divisor == -1){
                if( dividend == INT_MIN){
                    return INT_MAX;
                }
                return -dividend;
            } 
            //判断符号  
            int flag = 1;

            if((dividend > 0 && divisor < 0)||(dividend < 0 && divisor > 0)){
                flag = -1;
            }
            //全都变为负数,防止溢出
            int a = dividend > 0 ? -dividend : dividend;
            int b = divisor > 0 ? -divisor : divisor;
            //
            if( a > b ){
                return 0;
            }
            int res = div(a, b);
            return flag == 1 ? res : -res;

    }
};
```

### 知识点：

第一种思路：

不断比较2^n 与 divisior ，找到最接近的n，之后利用循环 （dividend- 2^n )  - divisior;

第二种思路：

利用二分法和递归。

比如：

60/8 = (60-32)/8 + 4 = (60-32-16)/8 + 2 + 4 = 1 + 2 + 4 = 7

### 官方代码：

```
class Solution {
public:
    int divide(int dividend, int divisor) {
        if(dividend == 0) return 0;
        if(divisor == 1) return dividend;
        if(divisor == -1){
            if(dividend>INT_MIN) return -dividend;// 只要不是最小的那个整数，都是直接返回相反数就好啦
            return INT_MAX;// 是最小的那个，那就返回最大的整数啦
        }
        long a = dividend;
        long b = divisor;
        int sign = 1; 
        if((a>0&&b<0) || (a<0&&b>0)){
            sign = -1;
        }
        a = a>0?a:-a;
        b = b>0?b:-b;
        long res = div(a,b);
        if(sign>0)return res>INT_MAX?INT_MAX:res;
        return -res;
    }
    int div(long a, long b){  // 似乎精髓和难点就在于下面这几句
        if(a<b) return 0;
        long count = 1;
        long tb = b; // 在后面的代码中不更新b
        while((tb+tb)<=a){
            count = count + count; // 最小解翻倍
            tb = tb+tb; // 当前测试的值也翻倍
        }
        return count + div(a-tb,b);
    }
};
```

### 热评：

```
有没有和我一样看到位运算就怕的...
```

