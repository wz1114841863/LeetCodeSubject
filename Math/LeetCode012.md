### 题目：

罗马数字包含以下七种字符： `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000

例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
给你一个整数，将其转为罗马数字。

### 难度：

中等。

### 示例：

```
输入: num = 1994
输出: "MCMXCIV"
解释: M = 1000, CM = 900, XC = 90, IV = 4.
输入: num = 58
输出: "LVIII"
解释: L = 50, V = 5, III = 3.
```

### 解题思路：

```
初始想法：
对于不同的各种情形要考虑清楚。
建里一个map存储1000，900， 500， 400... 等每一位数字对应的罗马数字。
要重点考虑吧 4 和 9这两个数字。
1.获取数字， 1<= num <= 3999，获取 千、百、十、个位，如果没有就是0.
2.对每一位逐步进行处理。处理完获取的字符加入到字符串最后面。
3.返回字符串。、

错误改进：
发现用map并不合适，没法去单纯的获取键值，说明这里选择map并不合适
甚至比不上单纯的使用数字。所以需要改为别的数据结构
```

### 代码实现：

```c++
class Solution {
public:
    string intToRoman(int num) {
        /* 用map并不合适。需要改进。
            //初始化MAP。讨论所有可能出现的情况。
            map<int, string> ROMAN={
                {1000,"M"}, {900, "CM"},{500,"D"},{400,"CD"},{100,"C"},{90,"XC"},
                {50,"L"},{40,"XL"},{10,"X"},{9,"IX"},{5,"V"},{4,"IV"},{1,"I"},
            }
        */
        //改用pair数组，其实相当于使用两个数组。
        pair<int, string> ROMAN[] ={
            {1000,"M"}, {900, "CM"},{500,"D"},{400,"CD"},{100,"C"},{90,"XC"},
            {50,"L"},{40,"XL"},{10,"X"},{9,"IX"},{5,"V"},{4,"IV"},{1,"I"},
        };
        //返回的字符串。
        string result;
        pair<int, string> roman;
        for(int i=0; i<13; i++){
            roman = ROMAN[i];
             while (num >= roman.first) {
                num -= roman.first;
                result += roman.second;
            }
            if (num == 0) {
                break;
            }
        }
        return result;
    }
};
```

### 知识点：

需要考虑不同容器的使用情况。

底层容器：

vector, list, set，map。

容器衍生器：

queue， stack。

还有：

数组， pair<>

### 官方代码：

```
官方代码：
const pair<int, string> valueSymbols[] = {
    {1000, "M"},
    {900,  "CM"},
    {500,  "D"},
    {400,  "CD"},
    {100,  "C"},
    {90,   "XC"},
    {50,   "L"},
    {40,   "XL"},
    {10,   "X"},
    {9,    "IX"},
    {5,    "V"},
    {4,    "IV"},
    {1,    "I"},
};

class Solution {
public:
    string intToRoman(int num) {
        string roman;
        for (const auto &[value, symbol] : valueSymbols) {
            while (num >= value) {
                num -= value;
                roman += symbol;
            }
            if (num == 0) {
                break;
            }
        }
        return roman;
    }
};
```

### 热评：

```
写了一长串把自己都看笑了
```

```
我不管，if else是这道题最棒的解法（叉腰）！
```

