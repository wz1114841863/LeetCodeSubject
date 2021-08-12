### 题目：

给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。

你可以按 **任何顺序** 返回答案。

### 难度：

中等。

### 示例：

```
输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

### 解题思路：

```
递归：判断剩余数是否大于等于k-1，
边界条件：剩余数小于k-1 已经 实现了n个数的组合、
循环遍历：取出第一个数，剩余的n-1个数中获取 k-1 个数。
```

### 代码实现：

```c++
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        //返回结果
        vector<vector<int>> res;
        //中间向量
        vector<int> temp;
        //特殊情况
        if( n == k ){
            for(int i=1; i<=n; i++){
                temp.push_back(i);
            } 
            res.push_back(temp);
            return res;  
        }
        //调用函数
        arrayPrint(n, k, res, temp);
        return res;
    }
private:
    void arrayPrint(int n, int k, vector<vector<int>> &res, vector<int> &temp){
        //边界条件
        if( n < k){
            return ;
        }
        if( k == 0 ){
            res.push_back(temp);
            return ;
        }
        //循环遍历
        //n > k
        for(int i=n; i>0; i--){
            temp.push_back(i);
            arrayPrint(i-1, k-1, res, temp);
            temp.pop_back();
        }
        
        return;
    }
};
```

### 知识点：

难点在于递归结束的条件和剪枝。避免没必要的步骤。

### 官方代码：

```
class Solution {
public:
    vector<int> temp;
    vector<vector<int>> ans;

    void dfs(int cur, int n, int k) {
        // 剪枝：temp 长度加上区间 [cur, n] 的长度小于 k，不可能构造出长度为 k 的 temp
        if (temp.size() + (n - cur + 1) < k) {
            return;
        }
        // 记录合法的答案
        if (temp.size() == k) {
            ans.push_back(temp);
            return;
        }
        // 考虑选择当前位置
        temp.push_back(cur);
        dfs(cur + 1, n, k);
        temp.pop_back();
        // 考虑不选择当前位置
        dfs(cur + 1, n, k);
    }

    vector<vector<int>> combine(int n, int k) {
        dfs(1, n, k);
        return ans;
    }
};
```

### 热评：

```
只会这么写，可以去捡工牌吗？
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        return list(itertools.combinations(range(1,n+1),k))
```

