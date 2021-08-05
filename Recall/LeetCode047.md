### 题目：

给定一个可包含重复数字的序列 `nums` ，**按任意顺序** 返回所有不重复的全排列。

### 难度：

中等。

### 示例：

```
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

### 解题思路：

```
    修改上一题的代码，去重。
    先排序，便于操作。
    1.同样的数字第二次出现，跳过循环，不执行排列
    2.最后剩余同样的数字，仅插入一次。
```

### 代码实现：

```c++
class Solution {
private:
    void arrange(vector<vector<int>> &res, const vector<int> &nums, vector<int> &temp){
        //获取nums的长度
        int len1 = nums.size();
        if( len1 == 2){
            //相等只插入一次，不相等，则插入两次。
            temp.push_back(nums[0]);  temp.push_back(nums[1]);
            res.push_back(temp);
            temp.pop_back();  temp.pop_back();
            if( nums[0] != nums[1]){
                temp.push_back(nums[1]);  temp.push_back(nums[0]);
                res.push_back(temp);
                temp.pop_back();  temp.pop_back();
            }

        }
        // n > 2, 调用 n-1
        //如何传递 n - 1 这样以一个数组
        vector<int> tempNums;
        for( int i = 0; i <= len1 - 1; i++){
            if( i != 0 && nums[i] == nums[i-1]){
                continue;
            }
            tempNums = nums;
            temp.push_back(nums[i]);
            tempNums.erase( tempNums.begin()+i);
            arrange(res, tempNums, temp);
            temp.pop_back();
        }
    }    
public:
    vector<vector<int>> permuteUnique(vector<int>& nums)  {
        //获取数组长度
        int len =  nums.size();
        //最终返回结果
        vector<vector<int>> res;
        //判断特殊情况
        if( len == 1 ){
            res.push_back(nums);
            return res;
        }
        //排序
        sort(nums.begin(), nums.end());
        //数组暂存
        vector<int> temp;
        //调用递归函数，传递res 和 nums
        arrange(res, nums, temp);
        return res;
    }
};
```

### 知识点：

我感觉自己的思路还是蛮清楚的，但也有可以优化的地方。

跟官方思路差不多。

### 官方代码：

```
class Solution {
    vector<int> vis;

public:
    void backtrack(vector<int>& nums, vector<vector<int>>& ans, int idx, vector<int>& perm) {
        if (idx == nums.size()) {
            ans.emplace_back(perm);
            return;
        }
        for (int i = 0; i < (int)nums.size(); ++i) {
            if (vis[i] || (i > 0 && nums[i] == nums[i - 1] && !vis[i - 1])) {
                continue;
            }
            perm.emplace_back(nums[i]);
            vis[i] = 1;
            backtrack(nums, ans, idx + 1, perm);
            vis[i] = 0;
            perm.pop_back();
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<vector<int>> ans;
        vector<int> perm;
        vis.resize(nums.size());
        sort(nums.begin(), nums.end());
        backtrack(nums, ans, 0, perm);
        return ans;
    }
};

```

### 热评：

```
用set其实可以无脑解决大量的去重问题。但是击败1%让我留下眼泪。
```

