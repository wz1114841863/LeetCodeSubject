### 题目：

将一个给定字符串 `s` 根据给定的行数 `numRows` ，以从上往下、从左到右进行 Z 字形排列。

(或许说V字形更合适)

### 难度：

中等

### 示例：

```
输入：s = "PAYPALISHIRING", numRows = 4
输出："PINALSIGYAHRPI"
解释：
P     I    N
A   L S  I G
Y A   H R
P     I
输入：s = "PAYPALISHIRING", numRows = 3
输出："PAHNAPLSIIGYIR"
解释：
P   A   H   N
A P L S I I G
Y   I   R
```

### 解题思路：

```
如果字符串长度小于行数或仅有一行直接返回。
根据规律取出逐行取出字符。
第一行和最后一行特别处理。
其余行一起处理。
```

### 代码实现：

```c++
class Solution {
public:
    string convert(string s, int numRows) {
        //字符串的长度小于numRows或只有一行时，直接返回字符串。
        if((s.size() <= numRows) ||(numRows == 1)){
            return s;
        }
        //创建新的字符串
        string str;
        //处理第一行，每个字符间隔（numRows-1) + (numRows-2) = 2*numRows -3.
        //在后移一位。= 2*numRows -3 + 1 = 2*numRows -2.
        for(int i=0; i < s.size(); i= i + 2 * numRows - 2){
            str = str + s[i];
        }
        //处理第二行 到第 numRows - 1行.每 k (k>= 2 && k <= numRows - 1)行的间隔为:
        //第一种：
        //(numRows - k) + (numRows - k -1) = 2*numRows - 2k -1
        //在后移一位。= 2*numRows - 2k -1 = 2*numRows - 2k.
        //第二种：
        //(k -1) + （k-2） = 2k-3;
        //再往后以移动一位。= 2k - 2 =2 * （k-1）.
        int flag;
        for(int k=2; k < numRows; k++){
            flag = 0;
            for(int i= k-1; i< s.size();){
                str = str + s[i];
                if(flag == 0){
                    i = i + 2*numRows - 2*k;
                    flag = 1;
                }else if(flag == 1){
                    i = i + 2* (k-1);
                    flag = 0;
                }
            }
        }
        //处理最后一行，每个字符间隔（numRows-1) + (numRows-2) = 2*numRows -3.
        //在后移一位。= 2*numRows -3 + 1 = 2*numRows -2.        
        for(int i=numRows-1; i<s.size(); i= i + 2 * numRows - 2){
            str = str + s[i];
        }
        return str;
    }
};
```

### 知识点：

应该来说只是找规律，并没有什么难度，但是跟官方代码相比，发现即使思路相同，但是代码的简洁性相差甚远。

”你还查的远呢.jpg“

另外一种解法是通过从左向右迭代字符串，确定字符位于 Z 字形图案中的哪一行。从左到右迭代 s，将每个字符添加到合适的行。可以使用当前行和当前方向这两个变量对合适的行进行跟踪。只有当向上移动到最上面的行或向下移动到最下面的行时，当前方向才会发生改变。

还有一种解法是创建二维数组或向量。但相比来说更为复杂而且浪费空间，意义不大。

### 官方代码：

```
class Solution {
public:
    string convert(string s, int numRows) {

        if (numRows == 1) return s;

        string ret;
        int n = s.size();
        int cycleLen = 2 * numRows - 2;

        for (int i = 0; i < numRows; i++) {
            for (int j = 0; j + i < n; j += cycleLen) {
                ret += s[j + i];
                if (i != 0 && i != numRows - 1 && j + cycleLen - i < n)
                    ret += s[j + cycleLen - i];
            }
        }
        return ret;
    }
};
```

### 热评：

```
生气是无能的表现，这道题真得让我很生气！！！！
```

```
感觉自己是个智障
```

