### 题目：

给你一个整数 `columnNumber` ，返回它在 Excel 表中相对应的列名称。

```
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
```

### 难度：

简单

### 示例：

```
输入：columnNumber = 701
输出："ZY"
```

### 解题思路：

```
    不断对26取商和余，添加对应字符到字符串开头。

    如果余数不为零：直接取对应字符。
    如果余数为零，商减一，字符串前直接加个Z；
    商不为零？  返回前两步。
```

### 代码实现：

```c++
class Solution {
public:
    string convertToTitle(int columnNumber) {
        string s1 = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
        //返回结果
        string str;
        //余数和商
        int rem, mer;
        while(columnNumber != 0){
            rem = columnNumber % 26;
            mer = columnNumber / 26;
            if( rem != 0 ){
                str = s1[rem - 1] + str;
            }else{
                str = (string)"Z" + str;
                mer--;
            }
            columnNumber = mer;
        }   
        return str;
    }
};
```

### 知识点：

不是简单的26进制，因为没有零这个数字。

### 官方代码：

```
官方代码：
class Solution {
public:
    string convertToTitle(int columnNumber) {
        string ans;
        while (columnNumber > 0) {
            --columnNumber;
            ans += columnNumber % 26 + 'A';
            columnNumber /= 26;
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

### 热评：

```
0真是个伟大的数字啊；
```

