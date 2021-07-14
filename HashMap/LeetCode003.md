### 题目：

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

### 难度：

中等

### 示例：

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

### 解题思路：

1.滑动窗口。

利用两个指针，左指针指向子串最前端，右指针不断向前移动。(实际并没右使用指针，使用的是字符索引)

右指针不断向右移动，每一次向右移动，暂存字符长度就加一。

若未碰到重复字符，将暂存长度复制给length，因为暂存长度势必比length要打=大，即（tmp >= length)。

若碰到重复字符,根据约束条件，左侧指针不断移动直至所重复字符的位置（去掉重复字符）。

利用set存储当前字符。

重复以上步骤。

注意：

本来打算使用map来存储 <toil, s[toil]>， 但是map使用值来查找键时就需要自己写对应的函数，同时需要考虑取值等一系列问题，所以最终使用set，带来的缺点就是左指针需要逐步向前移动。

### 代码实现：

```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        //子串长度
        int length = 0, tmp = 0;
        //子串头位置坐标。
        int head = 0;
        //Map 用来存储哈希表，用来记录存放的字符索引及位置。
        unordered_set<char>  chars;
        //主循环,toil 即为尾指针。
        for(int toil=0; toil<s.size(); toil++){
            
            //一旦移动就+1。
            tmp++;
            if( !chars.count(s[toil] )){
                //还可以使用chars.find(s[toil]) == chars.end()              
                //存在就比较当前子串长度与暂存值
                length = max(length, tmp);                
            }
            else{
                //查找重复位置，移动head指针。
                while((head < toil)&&(chars.count(s[toil])  == 1 )){
                    chars.erase(s[head]);
                    head++;
                    tmp--;
                }

            }
            chars.insert( s[toil] );
            //测试输出。
            //cout << toil << " " << tmp << " " << length << " " << head << endl;

        }
        return length;

    }
};
```

### 知识点：

#### 1.set 与 map的联系与区别：

联系：

```
1.都是关联式容器。

2.都支持容器所支持的方法，例如find, set, insert...。

3.都以红黑树作为底层容器。

4.不允许键重复。
```

区别：

```
set里面每个元素只存有一个key值，它支持高效的关键字查询操作。只有键没有值。
```

```
map是一种key(键),value(值)的形式，用来保存键和值组成的集合，键必须是唯一的，但值可以不唯一。里面的元素可以根据键进行自动排序，由于map是key_value的形式，所以map里的所有元素都是pair类型。pair里面的first被称为key(键），second被称为value(值）。它可以通过关键字查找映射关联信息value，同时根据key值进行排序。
```

#### 2.算法效率：

虽然运行结果正确，但效率较低。以后还要改进。

### 官方代码：

```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        // 哈希集合，记录每个字符是否出现过
        unordered_set<char> occ;
        int n = s.size();
        // 右指针，初始值为 -1，相当于我们在字符串的左边界的左侧，还没有开始移动
        int rk = -1, ans = 0;
        // 枚举左指针的位置，初始值隐性地表示为 -1
        for (int i = 0; i < n; ++i) {
            if (i != 0) {
                // 左指针向右移动一格，移除一个字符
                occ.erase(s[i - 1]);
            }
            while (rk + 1 < n && !occ.count(s[rk + 1])) {
                // 不断地移动右指针
                occ.insert(s[rk + 1]);
                ++rk;
            }
            // 第 i 到 rk 个字符是一个极长的无重复字符子串
            ans = max(ans, rk - i + 1);
        }
        return ans;
    }
};
```

### 热评：

```
我的代码击败了全国百分之五的人，我想知道谁被我击败了。
```

```
第一眼看这题的反应，（我以前做过这个题好几遍了）。再看一眼之后发现，（什么？我什么时候做过这个题？）
```

