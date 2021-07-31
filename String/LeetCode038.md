### 题目：

给定一个正整数 n ，输出外观数列的第 n 项。

「外观数列」是一个整数序列，从数字 1 开始，序列中的每一项都是对前一项的描述。

你可以将其视作是由递归公式定义的数字字符串序列：

countAndSay(1) = "1"
countAndSay(n) 是对 countAndSay(n-1) 的描述，然后转换成另一个数字字符串。

### 示例：

```
1.     1
2.     11
3.     21
4.     1211
5.     111221
第一项是数字 1 
描述前一项，这个数是 1 即 “ 一 个 1 ”，记作 "11"
描述前一项，这个数是 11 即 “ 二 个 1 ” ，记作 "21"
描述前一项，这个数是 21 即 “ 一 个 2 + 一 个 1 ” ，记作 "1211"
描述前一项，这个数是 1211 即 “ 一 个 1 + 一 个 2 + 二 个 1 ” ，记作 "111221"
输入：n = 1
输出："1"
解释：这是一个基本样例。
```

### 解题思路：

```
递归，利用字符串存储返回时变为数字。
从前往后遍历数组，判断下一字符是否与当前字符相同
    相同，接着后移。
    不同，记录次数，记录当前数字，拼接字符串。
```

### 代码实现：

```c++
class Solution {
public:
    string countAndSay(int n) {
        //初始向量
        vector<string> countStr;
        string temp;
        temp = "1";
        countStr.push_back(temp);
        temp = "11";
        countStr.push_back(temp);
        //判断特殊情况
        if( n == 1 || n == 2){
            return countStr[ n-1 ];
        }
        //遍历
        string str1;
        str1 = "";
        int count;
        for(int i=2; i<n; i++){
            temp = countStr[ i-1 ];
            str1 = "";
            for(int j=0; j<temp.size(); j++){
                cout << 2 << endl;
                count = 1;
                while((j+1 < temp.size()) && (temp[j] == temp[j+1])){
                    count++;
                    j++;
                }
                cout << 3 << endl;
                str1 += to_string(count);
                str1 += temp[j];
            }
            countStr[ i ] = str1;
        }
        return countStr[ n-1 ];
    }
};
```

### 知识点：

字符串拼接。

int 与 string 互转。

### 优秀范例：

```
class Solution {
public:
    string countAndSay(int n) {
        if(n == 1)
            return "1";    // f(1) = 1
        
        string res = "1";  // f(1) = 1, 作为迭代的初始值放入到结果中
        for(int i=0; i<n-1; i++)
        {
            string currentCombinedStr = "";
            char curFirstChar = res[0];    // 存放当前分片的第一个字符
            int currentCharCount = 0;            
            for(char ch : res)             // 将当前的字符与当前分片的第一个字符比较
            {
                if(ch == curFirstChar)
                    currentCharCount += 1;
                else {         
                    // 出现新的字符时，把已处理的连续相同字符的信息插入到结果字符串中
                    currentCombinedStr.append(to_string(currentCharCount));
                    currentCombinedStr.push_back(curFirstChar);

                    curFirstChar = ch;
                    currentCharCount = 1;
                }
            }

            // 把末尾连续相同字符的信息插入到结果字符串中(对末尾一段字符来说，不会再有新的字符了)
            currentCombinedStr.append(to_string(currentCharCount));
            currentCombinedStr.push_back(curFirstChar);            
            res = currentCombinedStr; // 将结果用作下一轮循环的初始值
        }

        return res;
    }
};
```

### 热评：

```
这种饶舌题目是出给嘻哈程序员的吗？
```

