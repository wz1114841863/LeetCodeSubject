### 题目：

编写一个函数，输入是一个无符号整数（以二进制串的形式），返回其二进制表达式中数字位数为 '1' 的个数（也被称为[汉明重量](https://baike.baidu.com/item/汉明重量)）。

### 难度：

简单。

### 示例：

```
输入：00000000000000000000000000001011
输出：3
解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
```

### 解题思路：

```
输入是整无符号整数，如何判断其二进制数对应有几个1.
与 2^n (n <= 32)比较，大于说明对应位置有1，减去对应数，且计数+1；
n--;
```

### 代码实现：

```c++
class Solution {
public:
    int hammingWeight(uint32_t num) {
        //返回结果
        int count = 0, n = 31;
        while(n >= 0){
            uint32_t temp = pow(2, n);
            if( num >= temp){
                cout << num << endl;
                count++;
                num = num -temp;
            }
            n--;
            if(num == 0)
                break;
        }
        return count;
    }
};
```

### 知识点：

这道题本意应该是考察位运算，结果我做成了暴力破解法。

### 官方代码：

```
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int ret = 0;
        while (n) {
            n &= n - 1;
            ret++;
        }
        return ret;
    }
};
```

### 热评：

```
//c++ 今年最流行的写法
return bitset<32>(n).count();
```

