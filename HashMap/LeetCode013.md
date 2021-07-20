### 题目：

罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。

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
给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

### 难度：

简单。

### 示例：

```
输入: "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.
输入: "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

### 解题思路：

```
这道题是上一道题的延申，
依次遍历每两个字符，查询map是否存在：
    存在，表示当前两个字符是一体的，i = i + 2;
    不存在，表示s[i]与s[i+1]不是一体的，但是不能判断是s[i+1]与s[i+2],所以 i = i + 1;
但同时要考虑边界情况，也就是最后一个字符，因此可以先在s的后面的加一个其余字符，比如字符‘0’
来保证遍历过程的统一性。
```

### 代码实现：

```c++
class Solution {
public:
    int romanToInt(string s) {
        //先给s后面加个字符'0'，这样遍历时就不用考虑边界情况了。
        s = s + '0';
        cout << s.size();
        //用map来存所有结果。用字符串做键，数字做值。
        map<string, int> ROMAN={
            {"M", 1000}, {"CM", 900}, {"D", 500}, {"CD", 400}, {"C", 100}, {"XC", 90},
            {"L", 50}, {"XL", 40}, {"X", 10}, {"IX", 9}, {"V", 5}, {"IV", 4},{"I", 1},
        };
        //最终返回结果。
        int num = 0;
        //暂存字符串
        string temp;
        //遍历字符串
        for(int i=0; i<s.size() - 1;i++){
            temp = s.substr(i,2); //s[i] + s[i+1]
            if(ROMAN.count(temp)){  
                //如果存在这一组合
                num = num + ROMAN[temp];
                i++;  //之后for循环会再次实现i++；也就是 i=i+2.
            }
            else{
                //不存在这一组合
                temp = s[i];
                num = num + ROMAN[temp];
            }
        }
        return num;
    }
};
```

### 知识点：

查看官方的解题思路可以看到，它是根据字符的顺序来判断是应该加还是减去这个数字。

通常情况下，罗马数字中小的数字在大的数字的右边。若输入的字符串满足该情况，那么可以将每个字符视作一个单独的值，累加每个字符对应的数值即可。

若存在小的数字在大的数字的左边的情况，根据规则需要减去小的数字。对于这种情况，我们也可以将每个字符视作一个单独的值，若一个数字右侧的数字比它大，则将该数字的符号取反。

### 官方代码：

```
class Solution {
private:
    unordered_map<char, int> symbolValues = {
        {'I', 1},
        {'V', 5},
        {'X', 10},
        {'L', 50},
        {'C', 100},
        {'D', 500},
        {'M', 1000},
    };

public:
    int romanToInt(string s) {
        int ans = 0;
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            int value = symbolValues[s[i]];
            if (i < n - 1 && value < symbolValues[s[i + 1]]) {
                ans -= value;
            } else {
                ans += value;
            }
        }
        return ans;
    }
};
```

### 热评：

```
s = s.replace("IV","a");
s = s.replace("IX","b");
s = s.replace("XL","c");
s = s.replace("XC","d");
s = s.replace("CD","e");
s = s.replace("CM","f");
```

