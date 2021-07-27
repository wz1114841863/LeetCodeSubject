### 题目：

给你两个字符串 haystack 和 needle ，请你在 haystack 字符串中找出 needle 字符串出现的第一个位置（下标从 0 开始）。如果不存在，则返回  -1 。

说明：

当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与 C 语言的 strstr() 以及 Java 的 indexOf() 定义相符

### 难度：

简单

### 示例：

```
输入：haystack = "hello", needle = "ll"
输出：2
```

### 解题思路：

```
经典字符串匹配问题：KMP算法实现
首先排除特殊情况。
可以看到如果needle为空，返回0.
未匹配，返回-1.
匹配返回出现的第一个位置的索引。
```

### 代码实现：

```c++
class Solution {
public:
    int strStr(string haystack, string needle) {
        // 获取字符串长度。
        int n = haystack.size();
        int m = needle.size();
        //讨论特殊情况
        if( m == 0){
            return 0;
        }else if( n < m){
            return -1;
        }
        //处理一下字符串，使遍历条件统一
        //设置哨兵
        haystack.insert(haystack.begin(), ' ');
        needle.insert(needle.begin(), ' ');
        //创建和预处理next数组
        vector<int> next(m + 1);
        next[0] = next[1] = 0;
        for(int i=2, j=0; i < m+1; i++){
            while( j && needle[i] != needle[ j+1 ]){
                j = next[j];
            }
            if(needle[i] == needle[ j+1 ]){
                j++;
            }
            next[i] = j;
        }
        //匹配过程
        for(int i = 1, j = 0; i < n+1; i++){
            while(j && haystack[i] != needle[j + 1]){
                j = next[j];
            }
            if(haystack[i] == needle[j + 1]){
                 j++;                
            }
            if(j == m){
                return i - m;
            }
        }
        return -1;

    }
};
```

### 知识点：

对应简单难度的话就应该是暴力破解法，可是字符串匹配有经典的KMP算法等。

KMP算法思路：

**在「非完全匹配」的过程中提取到有效信息进行复用，以减少「重复匹配」的消耗。**

关键在于 next  数组的构造。

### 官方代码：

```
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = haystack.size(), m = needle.size();
        if (m == 0) {
            return 0;
        }
        vector<int> pi(m);
        for (int i = 1, j = 0; i < m; i++) {
            while (j > 0 && needle[i] != needle[j]) {
                j = pi[j - 1];
            }
            if (needle[i] == needle[j]) {
                j++;
            }
            pi[i] = j;
        }
        for (int i = 0, j = 0; i < n; i++) {
            while (j > 0 && haystack[i] != needle[j]) {
                j = pi[j - 1];
            }
            if (haystack[i] == needle[j]) {
                j++;
            }
            if (j == m) {
                return i - m + 1;
            }
        }
        return -1;
    }
};

```

### 热评：

```
无语，这种题目就算了吧。要是说考察寻找字串快的可以用到KMP等方法，难度应该要归类到困难。要是用各种语言的内置函数，又不知道作为一道算法题的意义何在？考察对语言的熟悉程度？差评！
```

