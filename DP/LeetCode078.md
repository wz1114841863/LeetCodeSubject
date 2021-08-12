### 题目：

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集

### 难度：

中等。

### 示例：

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

### 解题思路：

```
如果结合前一题的话，可以去调用(n, k)，其中k从1递增到n，再加上一个空集，但是复杂度会很高

第i个数组成的集合 等于 前i-1个数组成的集合 + 前 i - 1 个数组成的集合+i。
```

### 代码实现：

```c++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        //返回结果
        vector<vector<int>> res;
        //中间向量
        vector<int> temp;
        //空集是必有的
        res.push_back(temp);
        int lenRes;
        //获取nums长度
        int lenNums = nums.size();
        int num;
        //遍历
        for(int i=0; i<lenNums; i++){
            num = nums[i];
            lenRes = res.size();
            for(int j=0; j<lenRes; j++){
                temp = res[j];
                temp.push_back(num);
                //res在动态递增。
                res.push_back(temp);
            }
        }
        return res;
    }
};
```

### 知识点：

看了官方解答可以用递归。我都解法应该属于动态规划吧。

### 官方代码：

```
class Solution {
public:
    vector<int> t;
    vector<vector<int>> ans;

    void dfs(int cur, vector<int>& nums) {
        if (cur == nums.size()) {
            ans.push_back(t);
            return;
        }
        t.push_back(nums[cur]);
        dfs(cur + 1, nums);
        t.pop_back();
        dfs(cur + 1, nums);
    }

    vector<vector<int>> subsets(vector<int>& nums) {
        dfs(0, nums);
        return ans;
    }
};
```

### 热评：

```
字典排序思路好赞啊。
```

