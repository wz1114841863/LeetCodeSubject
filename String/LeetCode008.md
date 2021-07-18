### 题目：

请你来实现一个 myAtoi(string s) 函数，使其能将字符串转换成一个 32 位有符号整数（类似 C/C++ 中的 atoi 函数）。

函数 myAtoi(string s) 的算法如下：

读入字符串并丢弃无用的前导空格检查下一个字符（假设还未到字符末尾）为正还是负号，读取该字符（如果有）。 确定最终结果是负数还是正数。 如果两者都不存在，则假定结果为正。
读入下一个字符，直到到达下一个非数字字符或到达输入的结尾。字符串的其余部分将被忽略。
将前面步骤读入的这些数字转换为整数（即，"123" -> 123， "0032" -> 32）。如果没有读入数字，则整数为 0 。必要时更改符号（从步骤 2 开始）。如果整数数超过 32 位有符号整数范围 [−231,  231 − 1] ，需要截断这个整数，使其保持在这个范围内。具体来说，小于 −231 的整数应该被固定为 −231 ，大于 231 − 1 的整数应该被固定为 231 − 1 。返回整数作为最终结果。

```
注意：
本题中的空白字符只包括空格字符 ' ' 。
除前导空格或数字后的其余字符串外，请勿忽略 任何其他字符。
```

### 难度：

中等。

### 示例：

```
输入：s = "42"
输出：42
解释：加粗的字符串为已经读入的字符，插入符号是当前读取的字符。
第 1 步："42"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："42"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："42"（读入 "42"）
           ^
解析得到整数 42 。
由于 "42" 在范围 [-231, 231 - 1] 内，最终结果为 42 。

输入：s = "4193 with words"
输出：4193
解释：
第 1 步："4193 with words"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："4193 with words"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："4193 with words"（读入 "4193"；由于下一个字符不是一个数字，所以读入停止）
             ^
解析得到整数 4193 。
由于 "4193" 在范围 [-231, 231 - 1] 内，最终结果为 4193 。

```

### 解题思路：

```
主要是要考虑的情况比较多，本身实现没什么难度：
1.如果有前导空格需要处理前导空格。
2.判断符号：记录正负
3.处理数字、判断范围和边界条件。
注意：
1.连续，只有开头可以出现空格，+、-后面出现字符，数字之间出现字符全部作为终止条件。
2.数字应该是int型，注意取值范围和处理。
```

### 代码实现：

```c++
class Solution {
public:
    int myAtoi(string s) {
        //获取字符串长度
        int len = s.size();
        //cout << len << endl;
        //若为空直接返回。
        if(len == 0){
            return 0;
        }
        //索引指针
        int index = 0;
        //先处理前面的空格,先不管前面是否存在非法字符，总之先处理掉可能存在的空格
        while(s[index] == ' '){
            index++;
        }
        //判断当前字符是什么：+，-，还是非法字符，总之不可能是空格。
        int flag = 1; //1:表示正数，-1表示负数：
        if( s[index] == '+'){
            flag = 1;
            index++;
        }else if(s[index] == '-'){
            flag = -1;
            index++;
        }else if( (s[index] < '0') || s[index] > '9' ){
            return 0;
        }
        //此时index指向+、-符号后（如果存在）的第一个字符，若不存在，则已经指向第一个数字。
        //这里我选择使用long 型来存储数字，避免越界，同时要考虑正负号。最后转为整形即可。
        long number = 0;
        while((s[index] >= '0') && (s[index] <= '9')){
            number = number*10 + (int)(s[index] - '0');
            if ((number > INT_MAX) && (flag == 1)){
                return INT_MAX;
            }else if((flag*number <= INT_MIN)&&(flag == -1)){
                return INT_MIN;
            }
            index++;
        }
        //int 范围 -2147483648~214748364
        //cout << number << endl;
        return flag * (int)number;
    }
};
```

### 知识点：

 我个人的算法（或许都不能称作算法）只是考虑了所有可能的情况实现最后的结果。

但是官方给出了一种利用有限状态机来考虑的方法。

字符串处理的题目往往涉及复杂的流程以及条件情况，如果直接上手写程序，一不小心就会写出极其臃肿的代码。

因此，为了有条理地分析每个输入字符的处理方法，我们可以使用自动机这个概念：

我们的程序在每个时刻有一个状态 s，每次从序列中输入一个字符 c，并根据字符 c 转移到下一个状态 s'。这样，我们只需要建立一个覆盖所有情况的从 s 与 c 映射到 s' 的表格即可解决题目中的问题。

![](..\Images\DFA.jpg)

可以说是非常巧妙的思路。

初次之外，还可以利用正则表达式匹配来解决，更加简洁。

```
class Solution:
    def myAtoi(self, s: str) -> int:
        return max(min(int(*re.findall('^[\+\-]?\d+', s.lstrip())), 2**31 - 1), -2**31)
```

### 官方代码：

```
class Automaton {
    string state = "start";
    unordered_map<string, vector<string>> table = {
        {"start", {"start", "signed", "in_number", "end"}},
        {"signed", {"end", "end", "in_number", "end"}},
        {"in_number", {"end", "end", "in_number", "end"}},
        {"end", {"end", "end", "end", "end"}}
    };

    int get_col(char c) {
        if (isspace(c)) return 0;
        if (c == '+' or c == '-') return 1;
        if (isdigit(c)) return 2;
        return 3;
    }
public:
    int sign = 1;
    long long ans = 0;

    void get(char c) {
        state = table[state][get_col(c)];
        if (state == "in_number") {
            ans = ans * 10 + c - '0';
            ans = sign == 1 ? min(ans, (long long)INT_MAX) : min(ans, -(long long)INT_MIN);
        }
        else if (state == "signed")
            sign = c == '+' ? 1 : -1;
    }
};

class Solution {
public:
    int myAtoi(string str) {
        Automaton automaton;
        for (char c : str)
            automaton.get(c);
        return automaton.sign * automaton.ans;
    }
};

```

### 热评：

```
这道题的魅力在于他让你以为你自己会做，然后你提交了20几次都不会过。
```

```
天天学编译原理，结果就是想不起来这个DFA，傻乎乎的写if else。
```

