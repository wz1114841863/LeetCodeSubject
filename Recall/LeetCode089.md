### 题目：

格雷编码是一个二进制数字系统，在该系统中，两个连续的数值仅有一个位数的差异。

给定一个代表编码总位数的非负整数 n，打印其格雷编码序列。即使有多个不同答案，你也只需要返回其中一种。

格雷编码序列必须以 0 开头。

### 难度：

中等。

### 示例：

```
输入: 2
输出: [0,1,3,2]
解释:
00 - 0
01 - 1
11 - 3
10 - 2

对于给定的 n，其格雷编码序列并不唯一。
例如，[0,2,3,1] 也是一个有效的格雷编码序列。

00 - 0
10 - 2
11 - 3
01 - 1
```

### 解题思路：

```
格雷编码是通过递归产生的.n位的格雷编码是 (n-1)位的格雷编码前面分别加0和1产生的.
所以利用递归产生 n-1.
第n个数组等于 (前n-1个数产生的格雷码)  + (前n-1个数产生的格雷码的逆序 + 2^(n-1))

格雷编码是通过递归产生的.n位的二进制格雷编码,是 (n-1)位的二进制格雷编码前面分别加0和1产生的. 
所以利用递归产生 n-1位的十进制格雷码.
n位的十进制的格雷码等于:
 (前n-1个数产生的格雷码)  + (前n-1个数产生的格雷码的逆序 + 2^(n-1))
因为二进制前面加零不改变十进制数的大小,二进制最前面加1,就等于 自身加上 pow(2, n-1).
为什么要加上逆序呢?这样才能保证相邻近的数字满足格雷码的要求.
```

### 代码实现：

```c++
class Solution {
public:
    vector<int> grayCode(int n) {
        //返回结果
        vector<int> res;
        //排除特殊情况
        if( n == 0 ){
            res.push_back(0);
            return res;
        }
        //回溯
        res = calGrayCode(n);
        return res;
    }
private:
    vector<int> calGrayCode(int n){
        vector<int> res;
        if( n == 1){
            res.push_back(0);
            res.push_back(1);
            return res;
        }
        res = calGrayCode(n-1);
        int len = res.size();
        int temp;
        for(int i=len-1; i>=0; i--){
            temp = res[i] + pow(2, n-1);
            res.push_back(temp);
        }
        return res;
    }
};
```

### 知识点：

毕竟是电子院的学生啊，格雷码，so  easy。

### 官方代码：

```
class Solution:
    def grayCode(self, n: int) -> List[int]:
        res, head = [0], 1
        for i in range(n):
            for j in range(len(res) - 1, -1, -1):
                res.append(head + res[j])
            head <<= 1
        return res
```

### 热评：

```
中等难度题（×)

背答案题（√）
```

