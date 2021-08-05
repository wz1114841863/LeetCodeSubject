### 题目：

给定一个字符串数组，将字母异位词组合在一起。可以按任意顺序返回结果列表。

字母异位词指字母相同，但排列不同的字符串。

### 难度：

中等。

### 示例：

```
输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
输出: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

### 解题思路：

```
首先可能会出现，同样字母的组合， 比如“aae”."eaa". 
从头到尾进行一次遍历：
    排序后的结果通过map查找。
        如果有，插入到对应的向量中。
        如果没有，插入到map，创建新的向量。
```

### 代码实现：

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        //返回结果：
        vector<vector<string>> res;
        //获取向量长度
        int len = strs.size(); 
        //判断特殊情况
        if( len == 0 || len == 1){
            res.push_back(strs);
            return res;
        }
        //遍历
        //记录res中个数
        int count = 0;
        string temp;
        //复制一份用来排序
        vector<string> copyStrs = strs;
        vector<string> tempStrs;
        //map中int字段
        int loc = 0;
        //用来存储排序后的字符串
        map<string, int> map1;
        res.clear();
        for(int i=0; i < len; i++){
            //排序，如果是字母异位词则排序后相等。
            sort(copyStrs[i].begin(), copyStrs[i].end());
            //查找，存在获取位置。不存在插入，添加位置
            //测试代码
            //cout << "sort    " << copyStrs[i] << endl;
            if(map1.count(copyStrs[i])){
                loc = map1[copyStrs[i]];
                res[loc].push_back(strs[i]);
                //cout << "loc:  " << loc << endl;
            }else{
                //插入
                map1[copyStrs[i]] = count;
                tempStrs.push_back(strs[i]);
                res.push_back(tempStrs);
                tempStrs.pop_back();
                count++;
            }
            //cout << "tempStrs:  " << tempStrs.size() << endl;
        }
        //测试代码
        /*
        map<string,int>::iterator iter;
        for (iter = map1.begin();iter != map1.end(); iter++){
            cout << iter->first << "-" << iter->second << endl;
        }
        */
        return res;
    }
};
```

### 知识点：

C++的sort好强。

### 官方代码：

```
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> mp;
        for (string& str: strs) {
            string key = str;
            sort(key.begin(), key.end());
            mp[key].emplace_back(str);
        }
        vector<vector<string>> ans;
        for (auto it = mp.begin(); it != mp.end(); ++it) {
            ans.emplace_back(it->second);
        }
        return ans;
    }
};
```

### 热评：

```
在美版leetcode上看到大神的思路，用质数表示26个字母，把字符串的各个字母相乘，这样可保证字母异位词的乘积必定是相等的。其余步骤就是用map存储，和别人的一致了。（这个用质数表示真的很骚啊！！!）
```

