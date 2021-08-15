### 题目：

给你一个整数数组 nums ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。返回的解集中，子集可以按 任意顺序 排列。

### 难度：

中等。

### 示例：

```
输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```

### 解题思路：

```
如果nums[i]  !=  nums[i-1]
第i个数组成的集合 等于 前i-1个数组成的集合 + 前 i - 1 个数组成的集合+i。
如果nums[i]  ==  nums[i-1] 
第i个数组成的集合 等于 前i-1个数组成的集合 + 第 i - 1 个数组成的集合+i。

数组大小：
1  ->  2  ->   4 ->  8 -> ...
出错：
有一种情况不能满足：
    nums从前n(n >= 3)个数如果相同,会出现重复错误。

主要是 第i-1个数所组成的集合个数多少个？
    利用count记录第i-1个数所得到的集合个数。
```

### 代码实现：

```c++
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        //返回结果
        vector<vector<int>> res;
        vector<int> temp;
        //添加空数组
        res.push_back(temp);
        //获取长度
        int len = nums.size();
        //排除特殊情况
        if( len == 0 ){
            return res;
        }
        //遍历前排序
        sort(nums.begin(), nums.end());
        //遍历数组
        int n = 0, count = 0, count1;
        for(int i = 0; i<len; i++){
            n = res.size();
            if(i != 0 && nums[i] == nums[i-1]){
                count1 = count;
                count = 0;
                for(int j = n-1; j >= n - count1; j--){
                    //后 count 个数
                    temp = res[j];
                    temp.push_back(nums[i]);
                    res.push_back(temp);
                    count++;
                }
            }else{
                count = 0;
                for(int j = n-1 ; j >= 0 ; j--){
                    //全 n 个数
                    temp = res[j];
                    temp.push_back(nums[i]);
                    res.push_back(temp);
                    count++;
                }
            }
        }
        return res;
    }
};
```

### 知识点：

考虑清楚所有情况即可。

### 官方代码：

```
class Solution {
public:
    vector<int> t;
    vector<vector<int>> ans;

    vector<vector<int>> subsetsWithDup(vector<int> &nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        for (int mask = 0; mask < (1 << n); ++mask) {
            t.clear();
            bool flag = true;
            for (int i = 0; i < n; ++i) {
                if (mask & (1 << i)) {
                    if (i > 0 && (mask >> (i - 1) & 1) == 0 && nums[i] == nums[i - 1]) {
                        flag = false;
                        break;
                    }
                    t.push_back(nums[i]);
                }
            }
            if (flag) {
                ans.push_back(t);
            }
        }
        return ans;
    }
};
```

### 热评：

```
我人傻了，面向题解编程。。。
```

