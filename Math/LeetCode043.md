### 题目：

给定两个以字符串形式表示的非负整数 `num1` 和 `num2`，返回 `num1` 和 `num2` 的乘积，它们的乘积也表示为字符串形式。

### 难度：

中等。

### 示例：

```
输入: num1 = "123", num2 = "456"
输出: "56088"
```

### 解题思路：

```
模拟竖式相乘：
    先选取两个字符串中较长的一串，固定，将较短的字符串拆开，分别与较长的字符串相乘。
    得到的字符串加过尾部补对应的零。
    再将所有的字符串相加。
```

### 代码实现：

```c++
class Solution {
private:
    string strMultiply(string str1, char char1){
        //将char1转换为数字
        int num1 = (int)char1 - 48;
        int num2;
        //temp最大也就是 9*9 = 81
        int temp;
        //temp1 记录个位，temp2 记录十位。
        int temp1 = 0;
        int temp2 = 0;
        //获取长度
        int len = str1.size();
        //返回结果
        string res;
        //提出字符串的每一位，与mul相乘，记录存储个位同时记录进位。
        for(int i = len - 1; i >= 0; i--){
            num2 = (int)str1[i] - 48;
            temp = num1 * num2 + temp2;
            temp1 = temp % 10;
            temp2 = temp / 10;
            res = to_string(temp1) + res;
        }   
        //将最后一次进位加进去
        if( temp2 != 0){
            res = to_string(temp2) + res;
        }

        return res;
    }
    string strAdd( string str, string temp){
        //返回结果
        string res;
        //进位标志
        int flag = 0;
        int temp1, temp2;
        //获取较长的字符串
        string longStr;
        string shortStr;
        //获取两个字符串长度。
        int len1;
        int len2;
        //判断长度
        if( str.size() >= temp.size()){
            len1 = str.size();
            longStr = str;
            len2 = temp.size();
            shortStr =temp;
        }else{
            len1 = temp.size();
            longStr = temp;
            len2 = str.size();
            shortStr = str;
        }
        //起始位置，从后往前加
        flag = 0;
        //先将重合位相加
        int i = len1 - 1;
        int j = len2 - 1;
        for(; j >= 0; j--, i--){
            temp1 = (int)longStr[i] - 48;
            temp2 = (int)shortStr[j] - 48;
            temp1 = temp1 + temp2 + flag;
            flag = temp1 / 10;
            temp1 = temp1 % 10;
            res = to_string(temp1) + res;
        }
        //再加longstr的剩余部分
        for(; i >= 0; i--){
            temp1 = (int)longStr[i] - 48;
            temp1 = temp1 + flag;
            flag = temp1 / 10;
            temp1 = temp1 % 10;
            res = to_string(temp1) + res;           
        }
        if(flag == 1){
             res = to_string(flag) + res;           
        }
        return res;     
    }
public:
    string multiply(string num1, string num2) {
        //最终结果；
        string str;
        //排除特殊情况
        if(num1.size() == 0 || num2.size() == 0){
            return str;
        }
        if((int)num1[0] == 48 || (int)num2[0] == 48){
            return (string)"0";
        }
        //获取较长的字符串
        string longStr;
        string shortStr;
        //获取两个字符串长度。
        int len1;
        int len2;
        //判断长度
        if( num1.size() >= num2.size()){
            len1 = num1.size();
            longStr = num1;
            len2 = num2.size();
            shortStr = num2;
        }else{
            len1 = num2.size();
            longStr = num2;
            len2 = num1.size();
            shortStr = num1;
        }
        //从后往前依次相乘。总共会获得len2的字符串。
        //同时利用count计数，记录尾部要加零的个数。
        int count = 0;
        string s1;
        vector<string> temp;
        for(int i = len2 - 1; i >=0 ; i--){
            s1 = strMultiply(longStr, shortStr[i]);
            //末尾加零
            for(int j=0; j < count; j++){
                s1 = s1 + "0";
            }
            //count + 1；
            count++;
            //存储
            temp.push_back(s1);
        }
        /*
        //测试：输出所有字符串
        for(int i = 0; i <= len2 - 1; i++){
            cout << temp[i] << endl; 
        }
        */
        //将所有的字符串相加
        //从第一个字符串加到最后一个。字符串长度不同。
        str = temp[0];
        for(int i = 1; i <= temp.size() - 1; i++){
            
            str = strAdd(str, temp[i]);
            //测试
            /*
                cout << "result  " << str << endl;
            */

        }
        return str;
    }
};
```

### 知识点：

代码还是挺臃肿的。

测试案例为：“123456789”  和  ”987654321“ 时出错。发现是最后的进位没有考虑。

number = 0
temp = 0
for i in range(1, 9):
    temp = 12345678 * i
    for j in range(1,i):
        temp = temp * 10;
    number = number + temp
    print(number)

### 官方代码：

```
class Solution {
    public String multiply(String num1, String num2) {
        if (num1.equals("0") || num2.equals("0")) {
            return "0";
        }
        String ans = "0";
        int m = num1.length(), n = num2.length();
        for (int i = n - 1; i >= 0; i--) {
            StringBuffer curr = new StringBuffer();
            int add = 0;
            for (int j = n - 1; j > i; j--) {
                curr.append(0);
            }
            int y = num2.charAt(i) - '0';
            for (int j = m - 1; j >= 0; j--) {
                int x = num1.charAt(j) - '0';
                int product = x * y + add;
                curr.append(product % 10);
                add = product / 10;
            }
            if (add != 0) {
                curr.append(add % 10);
            }
            ans = addStrings(ans, curr.reverse().toString());
        }
        return ans;
    }

    public String addStrings(String num1, String num2) {
        int i = num1.length() - 1, j = num2.length() - 1, add = 0;
        StringBuffer ans = new StringBuffer();
        while (i >= 0 || j >= 0 || add != 0) {
            int x = i >= 0 ? num1.charAt(i) - '0' : 0;
            int y = j >= 0 ? num2.charAt(j) - '0' : 0;
            int result = x + y + add;
            ans.append(result % 10);
            add = result / 10;
            i--;
            j--;
        }
        ans.reverse();
        return ans.toString();
    }
}
```

### 热评：

```
希望写str(int(num1)*int(num2))的朋友真的从中学到了东西。
```

```
//偷偷发个 fft 版 (手动狗头）
class Solution {
public:
    using CP = complex <double>;
    
    static constexpr int MAX_N = 256 + 5;

    double PI;
    int n, aSz, bSz;
    CP a[MAX_N], b[MAX_N], omg[MAX_N], inv[MAX_N];

    void init() {
        PI = acos(-1);
        for (int i = 0; i < n; ++i) {
            omg[i] = CP(cos(2 * PI * i / n), sin(2 * PI * i / n));
            inv[i] = conj(omg[i]);
        }
    }

    void fft(CP *a, CP *omg) {
        int lim = 0;
        while ((1 << lim) < n) ++lim;
        for (int i = 0; i < n; ++i) {
            int t = 0;
            for (int j = 0; j < lim; ++j) {
                if((i >> j) & 1) t |= (1 << (lim - j - 1));
            }
            if (i < t) swap(a[i], a[t]);
        }
        for (int l = 2; l <= n; l <<= 1) {
            int m = l / 2;
            for (CP *p = a; p != a + n; p += l) {
                for (int i = 0; i < m; ++i) {
                    CP t = omg[n / l * i] * p[i + m];
                    p[i + m] = p[i] - t;
                    p[i] += t;
                }
            }
        }
    }

    string run() {
        n = 1;
        while (n < aSz + bSz) n <<= 1;
        init();
        fft(a, omg);
        fft(b, omg);
        for (int i = 0; i < n; ++i) a[i] *= b[i];
        fft(a, inv);
        int len = aSz + bSz - 1;
        vector <int> ans;
        for (int i = 0; i < len; ++i) {
            ans.push_back(int(round(a[i].real() / n)));
        }
        // 处理进位
        int carry = 0;
        for (int i = ans.size() - 1; i >= 0; --i) {
            ans[i] += carry;
            carry = ans[i] / 10;
            ans[i] %= 10;
        }
        string ret;
        if (carry) {
            ret += to_string(carry);
        }
        for (int i = 0; i < ans.size(); ++i) {
            ret.push_back(ans[i] + '0');
        }
        // 处理前导零
        int zeroPtr = 0;
        while (zeroPtr < ret.size() - 1 && ret[zeroPtr] == '0') ++zeroPtr;
        return ret.substr(zeroPtr, INT_MAX);
    }

    string multiply(string num1, string num2) {
        aSz = num1.size();
        bSz = num2.size();
        for (int i = 0; i < aSz; ++i) a[i].real(num1[i] - '0');
        for (int i = 0; i < bSz; ++i) b[i].real(num2[i] - '0');
        return run();
    }
};
```

