### 题目：

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。

### 难度：

简单。

### 示例：

```
输入：s = "()[]{}"
输出：true
输入：s = "{[]}"
输出：true
```

### 解题思路：

```
很明显是使用栈来解决。
左括号入栈。
右括号与栈顶元素进行匹配，匹配成功，栈顶元素出栈，否则直接返回。
```

### 代码实现：

```c++
class Solution {
public:
    bool isValid(string s) {
        //获取字符串长度
        int len = s.size();
        //排除特殊情况。如果len 是奇数直接返回false
        if(len % 2){
            cout << len << endl;
            return false;
        }
        //建立map
        //第一个map用来存储当前字符是应该入栈还是判断出栈。
        map<string, int> map1 = {
            {"(", 1}, {"{", 1}, {"[", 1},
            {")", 0}, {"}", 0}, {"]", 0},
        };
        //第二个map用来存储对应的括号
        map<string, string> map2 = {
            {")", "("}, {"}", "{"}, {"]", "["},
        };
        //建立栈，当满足匹配条件时，栈中的元素最多也就是len/2，一旦超过len/2，说明不匹配。
        //如果栈满直接返回false
        stack<string> brackets;
        //cout << brackets.empty()  << endl;
        string temp;
        for(int i=0;  i<len; i++){
            temp = s[i];
            if( map1[temp] == 1){
                brackets.push(temp);
                if(brackets.size() > (len / 2)){
                    return false;
                }
            }else{
                if(brackets.empty()){
                    return false;
                }
                if( map2[temp] == brackets.top()){
                    brackets.pop();
                }else{
                    return false;
                }
            }
        }
        if(!brackets.empty()){
            return false;
        }

        return true;
       
    }
};
```

### 知识点：

经典用栈解法。

### 官方代码：

```
class Solution {
public:
    bool isValid(string s) {
        int n = s.size();
        if (n % 2 == 1) {
            return false;
        }

        unordered_map<char, char> pairs = {
            {')', '('},
            {']', '['},
            {'}', '{'}
        };
        stack<char> stk;
        for (char ch: s) {
            if (pairs.count(ch)) {
                if (stk.empty() || stk.top() != pairs[ch]) {
                    return false;
                }
                stk.pop();
            }
            else {
                stk.push(ch);
            }
        }
        return stk.empty();
    }
};

```

### 热评：

```
今晚 bilibili 的笔试题
```

