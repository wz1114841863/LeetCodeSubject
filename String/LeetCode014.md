### 题目：

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

### 难度：

简单。

### 示例：

```
输入：strs = ["flower","flow","flight"]
输出："fl"
```

### 解题思路：

```
第一反应就是暴力破解法，从第一个字符开始比较。但是时间复杂度太高。
改进一下：
    判断向量长度，如果长度为0或1，直接返回。反之执行下一步。
    先比较strs[0] 与 strs[1],获取所有的公共字符。
    再一次比较剩余字符串，直到数组遍历完毕，或此时公共字符串已经为空。
  不过两种算法的时间复杂度其实是一样的。
  
```

### 代码实现：

```c++
class Solution {
public:
//字符串比较获取公共字符串函数
    string stringsCompare(string s1, string s2){
        //获取两个字符串的长度
        int len1 = s1.size();
        int len2 = s2.size();
        //最终返回的字符串
        string res;
        if(( len1 == 0) || (len2 == 0)){
            return res; //初始化为空。
        }
        for(int i=0; i < min(len1, len2); i++){
            if(s1[i] == s2[i]){
                res = res + s1[i];
            }
            else{
                return res;
            }
        }
        return res;
    }   
    string longestCommonPrefix(vector<string>& strs) {
        //最终返回结果。
        string res;
        //判断数组长度
        if( strs.size() == 0){
            return res;
        }
        else if( strs.size() == 1){
            return res = strs[0];
        }
        //strs.size() >= 2
        //先比较str[0],str[1].
        res = stringsCompare(strs[0],strs[1]);
        for(int i=2; i<strs.size(); i++){
            res =  stringsCompare(res,strs[i]);
            if( res.empty()){
                return res;
            }
        }
        return res;
    }
    
};
```

### 知识点：

可能正是因为无法避免的字符串比较导致没有什么更好的算法来实现时间复杂度的优化，所以才是简单题。

借助python各种函数实现的都是流氓。(笑)

### 官方代码：

```
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (!strs.size()) {
            return "";
        }
        string prefix = strs[0];
        int count = strs.size();
        for (int i = 1; i < count; ++i) {
            prefix = longestCommonPrefix(prefix, strs[i]);
            if (!prefix.size()) {
                break;
            }
        }
        return prefix;
    }

    string longestCommonPrefix(const string& str1, const string& str2) {
        int length = min(str1.size(), str2.size());
        int index = 0;
        while (index < length && str1[index] == str2[index]) {
            ++index;
        }
        return str1.substr(0, index);
    }
};
```

### 热评：

```
只会暴力解法的我留下了没有技术含量的泪
```

```
python两种让你拍大腿的解法，时间复杂度你想象不到，短小精悍。 1、利用python的max()和min()，在Python里字符串是可以比较的，按照ascII值排，举例abb， aba，abac，最大为abb，最小为aba。所以只需要比较最大最小的公共前缀就是整个数组的公共前缀
2、利用python的zip函数，把str看成list然后把输入看成二维数组，左对齐纵向压缩，然后把每项利用集合去重，之后遍历list中找到元素长度大于1之前的就是公共前缀
```

