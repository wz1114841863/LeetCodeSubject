### 题目：

给你一个字符串 `s`，由若干单词组成，单词前后用一些空格字符隔开。返回字符串中最后一个单词的长度。

**单词** 是指仅由字母组成、不包含任何空格字符的最大子字符串。

### 难度：

简单。

### 示例：

```
输入：s = "luffy is still joyboy"
输出：6
```

### 解题思路：

```
//从右向左遍历，排除空格和考虑只有一个单词的情况。
```

### 代码实现：

```c++
class Solution {
public:
    int lengthOfLastWord(string s) {
        int len = s.length()-1;
		int count = 0;

		for (int i = len; i >= 0; i--) {
			while (s[i] != ' ') {
				count++; i--;
				if (i<0 || s[i] == ' ')
					return count;
			}
		}

        return 0;
    }
};
```

### 知识点：

主要是考虑只有一个单词的情况。

### 优秀解答：

```
class Solution {
public:
    int lengthOfLastWord(string s) 
    {
        istringstream iStrStrm(s);
        string word;
        while(iStrStrm >> word);
        return word.size();
    }
};
```

### 热评：

```
简单题我重拳出击。
```

