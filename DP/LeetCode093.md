### 题目：

给定一个只包含数字的字符串，用以表示一个 IP 地址，返回所有可能从 s 获得的 有效 IP 地址 。你可以按任何顺序返回答案。

有效 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。

例如："0.1.2.201" 和 "192.168.1.1" 是 有效 IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 无效 IP 地址。

### 难度：

中等。

### 示例：

```
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]
```

### 解题思路：

```
    首先s的长度len满足：    len >= 4, len <= 12;
    递归：
        判断当前是第几部分;
        边界条件：
            如果是第四部分，判断剩余部分是否有效且长度    >0 && <= 3。
                是：加入字符向量。
                否：返回上一级，
        遍历条件：
            如果不是第四部分，依次加入3-1个合理字符串，
```

### 代码实现：

```c++
class Solution {
public:
    vector<string> restoreIpAddresses(string s) {
        //获取长度:
        int len = s.size();
        //返回结果：
        vector<string> res;
        string temp;
        //处理特殊情况:
        if( len < 4 || len > 12){
            return res;
        }
        //调用递归函数
        printIP(s, 0, 0, temp, res, len);
        return res;
    }
private:
    //grade: 0-3 级。 loc: 0-len-1 范围内; 
    void printIP(string &s, int grade, int loc, string &temp, vector<string> &res, int &len){
        int lenTemp = temp.size();
        //边界条件处理
        //判断剩余字符师傅足够每个部分至少一个
        if((len - loc < 4 - grade )){
            //不够，返回
            return ;
        }
        //最后一级
        if(grade == 3){
            //判断剩余字符串是否合法
            string temp1 = s.substr(loc, len - loc);
            int len1 = temp1.size();
            if(len1 == 1 || s[loc] != (char)'0'){
                if(len1 < 3 || stoi(temp1) <= 255){
                    temp = temp + temp1;
                    res.push_back(temp);
                    temp = temp.substr(0, lenTemp);
                }
            }

            return ;
        }
        //grade: 0 - 2;
        if((char)s[loc] != (char)'0'){
            for(int i=3; i>0; i--){
                //判断长度是否足够
                if( loc + i < len){
                    string temp2 = s.substr(loc, i);
                    int len2 = temp2.size();
                    if(len2 < 3 || stoi(temp2) <= 255){
                        temp = temp + temp2 + (string)".";
                        printIP(s, grade + 1, loc + i, temp, res, len);
                        temp = temp.substr(0, lenTemp);
                    }
                    
                }    
            }
        }else{
            temp = temp + (string)"0" + (string)".";
            printIP(s, grade + 1, loc + 1, temp, res, len);
            temp = temp.substr(0, lenTemp);
        }

        return ;
    }
};
```

### 知识点：

还是需要讨论很多的情况，细心就行。

### 官方代码：

```
class Solution {
private:
    static constexpr int SEG_COUNT = 4;

private:
    vector<string> ans;
    vector<int> segments;

public:
    void dfs(const string& s, int segId, int segStart) {
        // 如果找到了 4 段 IP 地址并且遍历完了字符串，那么就是一种答案
        if (segId == SEG_COUNT) {
            if (segStart == s.size()) {
                string ipAddr;
                for (int i = 0; i < SEG_COUNT; ++i) {
                    ipAddr += to_string(segments[i]);
                    if (i != SEG_COUNT - 1) {
                        ipAddr += ".";
                    }
                }
                ans.push_back(move(ipAddr));
            }
            return;
        }

        // 如果还没有找到 4 段 IP 地址就已经遍历完了字符串，那么提前回溯
        if (segStart == s.size()) {
            return;
        }

        // 由于不能有前导零，如果当前数字为 0，那么这一段 IP 地址只能为 0
        if (s[segStart] == '0') {
            segments[segId] = 0;
            dfs(s, segId + 1, segStart + 1);
        }

        // 一般情况，枚举每一种可能性并递归
        int addr = 0;
        for (int segEnd = segStart; segEnd < s.size(); ++segEnd) {
            addr = addr * 10 + (s[segEnd] - '0');
            if (addr > 0 && addr <= 0xFF) {
                segments[segId] = addr;
                dfs(s, segId + 1, segEnd + 1);
            } else {
                break;
            }
        }
    }

    vector<string> restoreIpAddresses(string s) {
        segments.resize(SEG_COUNT);
        dfs(s, 0, 0);
        return ans;
    }
};
```

### 热评：

```
题挺简单，但是爷是条懒狗，所以还是cv了。
```

```
System.out.println("我选择放弃！！");
```

