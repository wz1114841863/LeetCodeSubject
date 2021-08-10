### 题目：

给你两个二进制字符串，返回它们的和（用二进制表示）。

输入为 **非空** 字符串且只包含数字 `1` 和 `0`。

### 难度：

简单。

### 示例：

```
输入: a = "1010", b = "1011"
输出: "10101"
```

### 解题思路：

```
模拟二进制加法
```

### 代码实现：

```c++
class Solution {
public:
    string addBinary(string a, string b) {
        //返回结果
        string res;
        //获取长度
        int la=a.length(),lb=b.length(),lm=max(la,lb);
        //加前导零对齐
        for(int i=la;i<lm;i++) a="0"+a;
        for(int i=lb;i<lm;i++) b="0"+b;
        //进位
        int carry=0;
        //模拟全加器，分别计算结果和进位值
        for(int i=lm-1;i>=0;i--){
            /*两位是a,b,上一位进位是c0,这一位进位是c,结果是s
            **s=a^b^c0
            **c=a&b|(c0&(a^b))
            */
            char temp=(a[i]-'0')^(b[i]-'0')^(carry)+'0';
            carry=((a[i]-'0')&(b[i]-'0'))|(carry&((a[i]-'0')^(b[i]-'0')));
            res=temp+res;
        }
        //如果算完还有进位，需要补个1
        if(carry==1)
            res="1"+res;
        return res;
    }
};
```

### 知识点：

除此之外，python还能转化为数字进行计算。

### 官方代码：

```
class Solution {
public:
    string addBinary(string a, string b) {
        string ans;
        reverse(a.begin(), a.end());
        reverse(b.begin(), b.end());

        int n = max(a.size(), b.size()), carry = 0;
        for (size_t i = 0; i < n; ++i) {
            carry += i < a.size() ? (a.at(i) == '1') : 0;
            carry += i < b.size() ? (b.at(i) == '1') : 0;
            ans.push_back((carry % 2) ? '1' : '0');
            carry /= 2;
        }

        if (carry) {
            ans.push_back('1');
        }
        reverse(ans.begin(), ans.end());

        return ans;
    }
};
```

### 热评：

```
return bin(int(a,2)+int(b,2))[2:]
```

