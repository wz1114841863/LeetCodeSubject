### 题目：

给你一个整数 `n` ，求恰由 `n` 个节点组成且节点值从 `1` 到 `n` 互不相同的 **二叉搜索树** 有多少种？返回满足题意的二叉搜索树的种数。

### 难度：

中等。

### 示例：

```
输入：n = 3
输出：5
```

### 解题思路：

```
    数目相同，排序个数相同。
    从 0-n遍历：
        i = m;
        获取前 0 到 m-1 个数的排列数;
        获取后 m+ 到 n 个数的排列数;
        相乘。
        累加
    返回结果
```

### 代码实现：

```c++
class Solution {
public:
    int numTrees(int n) {
        //记录n对应的种数
        // n==1 || n==2的情况
        // n == 时存为 1，表示空指针。
        vector<int> res{1,1,2};
        //处理特殊情况：
        if( n == 1 || n == 2){
            return res[ n ];
        }
        //遍历n
        int left = 0, right = 0, temp = 0, sum = 0;
        //从3 计算到 n
        for(int i=3; i<=n; i++){
            sum = 0;
            for(int j=1; j<=i; j++){
                //计算左边
                left = res[j-1];
                //计算右边
                right = res[i-j];
                //
                temp = left * right;
                sum = sum + temp;
            }
            //cout << sum << endl;
            res.push_back(sum);
        }
        return res[n];
    }   
};
```

### 知识点：

终于想出来解法了。感动到哭。

### 官方代码：

```
class Solution {
public:
    int numTrees(int n) {
        vector<int> G(n + 1, 0);
        G[0] = 1;
        G[1] = 1;

        for (int i = 2; i <= n; ++i) {
            for (int j = 1; j <= i; ++j) {
                G[i] += G[j - 1] * G[i - j];
            }
        }
        return G[n];
    }
};
```

### 热评：

```
解题思路：假设n个节点存在二叉排序树的个数是G(n)，1为根节点，2为根节点，...，n为根节点，当1为根节点时，其左子树节点个数为0，右子树节点个数为n-1，同理当2为根节点时，其左子树节点个数为1，右子树节点为n-2，所以可得G(n) = G(0)*G(n-1)+G(1)*(n-2)+...+G(n-1)*G(0)
```

```
class Solution {
public:
    int numTrees(int n) {
        switch(n){
            case 1: return 1;
            case 2: return 2;
            case 3: return 5;
            case 4: return 14;
            case 5: return 42;
            case 6: return 132;
            case 7: return 429;
            case 8: return 1430;
            case 9: return 4862;
            case 10: return 16796;
            case 11: return 58786;
            case 12: return 208012;
            case 13: return 742900;
            case 14: return 2674440;
            case 15: return 9694845;
            case 16: return 35357670;
            case 17: return 129644790;
            case 18: return 477638700;
            case 19: return 1767263190;
            default: return 0;
        }
    }
};
```

