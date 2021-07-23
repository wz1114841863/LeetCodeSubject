### 题目：

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

### 难度：

中等。

### 示例：

```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

### 解题思路：

```
理解题意，返回的字母组合中会包含每个数字提供的一个字符，所有返回的字符长度相同。
所以可以先考虑两个字母的所有字符组成的组合，得到结果。
将得到的结果与另一个字母对应的字符组合，得到新的中间结果。
依次，直到所有的字符遍历完毕。

利用map存储对应字符表。
编写函数完成字符匹配。
为了统一接口，使用vector<string>作为存储方式。
```

### 代码实现：

```c++
class Solution {
public:
    //函数功能，输入两个字符向量，返回一个包含所有组合的字符向量。
    vector<string> strComb(vector<string> str1, vector<string> str2){
        //返回结果。
        vector<string> res;
        string str;
        //字符处理, 组合两个字符串中的所有组合。
        for( int i=0; i<str1.size(); i++){
            for(int j=0; j<str2.size(); j++){
                str = str1[i] + str2[j];
                res.push_back(str);
            }
        }
        return res;
    }
    vector<string> letterCombinations(string digits) {
        //map存储对应字符表
        map<string, vector<string>> mapDigits = {
            {"2", {"a", "b", "c"}}, {"3", {"d", "e", "f"}}, {"4", {"g", "h", "i"}}, 
            {"5", {"j", "k", "l"}}, {"6", {"m", "n", "o"}}, {"7", {"p", "q", "r", "s"}}, 
            {"8", {"t", "u", "v"}}, {"9", {"w", "x", "y", "z"}},
        };
        //最终返回结果。
        vector<string> result, temp;
        //获取digits数组长度。
        int len = digits.size();
        //判断特殊情况。
        if( len == 0){
            return result;
        }else if( len == 1){
            return mapDigits[digits];
        }
        //  len >=2  && len <= 4
        //调用函数。
        string str1, str2;
        str1 = digits[0];
        str2 = digits[1];
        temp = strComb(mapDigits[str1], mapDigits[str2]);
        for(int i=2; i< len; i++){
            str2 = digits[i];
            temp = strComb(temp, mapDigits[str2]);
        }

        result = temp;
        return result;
    }
};
```

### 知识点：

回溯算法是对树形或者图形结构执行一次深度优先遍历，实际上类似枚举的搜索尝试过程，在遍历的过程中寻找问题的解。

深度优先遍历有个特点：当发现已不满足求解条件时，就返回，尝试别的路径。此时对象类型变量就需要重置成为和之前一样，称为「状态重置」。

许多复杂的，规模较大的问题都可以使用回溯法，有「通用解题方法」的美称。实际上，回溯算法就是暴力搜索算法，它是早期的人工智能里使用的算法，借助计算机强大的计算能力帮助我们找到问题的解。

回溯过程中维护一个字符串，表示已有的字母排列（如果未遍历完电话号码的所有数字，则已有的字母排列是不完整的）。该字符串初始为空。每次取电话号码的一位数字，从哈希表中获得该数字对应的所有可能的字母，并将其中的一个字母插入到已有的字母排列后面，然后继续处理电话号码的后一位数字，直到处理完电话号码中的所有数字，即得到一个完整的字母排列。然后进行回退操作，遍历其余的字母排列。

回溯算法用于寻找所有的可行解，如果发现一个解不可行，则会舍弃不可行的解。在这道题中，由于每个数字对应的每个字母都可能进入字母组合，因此不存在不可行的解，直接穷举所有的解即可。

### 官方代码：

```
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> combinations;
        if (digits.empty()) {
            return combinations;
        }
        unordered_map<char, string> phoneMap{
            {'2', "abc"},
            {'3', "def"},
            {'4', "ghi"},
            {'5', "jkl"},
            {'6', "mno"},
            {'7', "pqrs"},
            {'8', "tuv"},
            {'9', "wxyz"}
        };
        string combination;
        backtrack(combinations, phoneMap, digits, 0, combination);
        return combinations;
    }

    void backtrack(vector<string>& combinations, const unordered_map<char, string>& phoneMap, const string& digits, int index, string& combination) {
        if (index == digits.length()) {
            combinations.push_back(combination);
        } else {
            char digit = digits[index];
            const string& letters = phoneMap.at(digit);
            for (const char& letter: letters) {
                combination.push_back(letter);
                backtrack(combinations, phoneMap, digits, index + 1, combination);
                combination.pop_back();
            }
        }
    }
};

```

### 热评：

```
我战胜了5%的人 请问谁被我战胜了。
```

