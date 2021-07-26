### 题目：

数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

### 难度：

中等。

### 示例：

```
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
```

### 解题思路：

```
初始想法：
想要找规律，由 n-1 进而生成 n, 发现太难找到规律。(看了题解之后发现还是自己太菜，其实是有规律的。)
改变思路：
    已知n，那就是直到一共有 n 对括号。那是否可以对这个 n 对括号进行排列，实现所有的有效组合。
    任意组合的第一位肯定是 "(", 最后一位肯定是")"。
    主要是中间的排列组合。
    遵守的规则有：
        ")"的个数不能超过前面已有的"("的个数。
        "(" 和 ")" 的个数都为n
        每个最终字符的长度为 2n。
    利用递归实现(用left代替左括号的数目， right代替右括号的数目)：
        边界条件：
            left ==  right == 0；
        只能添加左括号的情况
            left == right != 0；
        既可以添加左括号， 又可以添加右括号的情况：
            0 <= left < right <= n    
```

### 代码实现：

```c++
class Solution {
public:
    vector<string> result;
    vector<string> generateParenthesis(int n) {
        //调用递归函数
        string str;
        addBrackets(str, n, n);
        return result;
    }
private:
    void addBrackets(string str, int left, int right){
        //边界条件，如果左右括号剩余数都为零。将当前结果添加到向量组中去，然后返回
        if((left == 0) && (right == 0)){
            result.push_back(str);
            return ;
        }
        //如果左括号数目等于右括号数目，只能再添加右括号。
        if( left == right){
            addBrackets(str + '(', left - 1, right);
        }else if(left < right){
            if(left > 0){
                addBrackets(str + '(', left - 1, right);
            }
            addBrackets(str + ')', left, right - 1);
        }
    }
};
```

### 知识点：

解题方法大致分为两种：

一是利用递归。

二是利用动态规划。

### 官方代码：

```
class Solution {
    void backtrack(vector<string>& ans, string& cur, int open, int close, int n) {
        if (cur.size() == n * 2) {
            ans.push_back(cur);
            return;
        }
        if (open < n) {
            cur.push_back('(');
            backtrack(ans, cur, open + 1, close, n);
            cur.pop_back();
        }
        if (close < open) {
            cur.push_back(')');
            backtrack(ans, cur, open, close + 1, n);
            cur.pop_back();
        }
    }
public:
    vector<string> generateParenthesis(int n) {
        vector<string> result;
        string current;
        backtrack(result, current, 0, 0, n);
        return result;
    }
};
```

### 热评：

```
做完每日一题，慢慢就放弃了转行的念头......
```

