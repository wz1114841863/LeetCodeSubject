### 题目：

给定一个无重复元素的正整数数组 candidates 和一个正整数 target ，找出 candidates 中所有可以使数字和为目标数 target 的唯一组合。

candidates 中的数字可以无限制重复被选取。如果至少一个所选数字数量不同，则两种组合是唯一的。 

对于给定的输入，保证和为 target 的唯一组合数少于 150 个。

### 难度：

中等。

### 示例：

```
输入: candidates = [2,3,6,7], target = 7
输出: [[7],[2,2,3]]
输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]
```

### 解题思路：

```
初始想法：
    递归。循环遍历。
    可以先对数组进行排序，方便进行后续判断。
    排序后从后往前查找。先找大的数.定位到小于等于target的那一位。
    调用函数，开始遍历。
    递归思想：
        边界条件：
            如果输入元素为0，res加入temp（一组解），弹出temp末尾元素，返回。
            如果输入元素小于最小值，弹出temp末尾元素，返回。
        剪枝 和 去重
            要判断的元素一定要小于target， 且位置一定要小于等于上一元素的位置。
        for循环递归调用
            调用自身。
```

### 代码实现：

```c++
class Solution {
private:
    //定义递归函数：
    //返回1代表成功查找。返回0代表失败。
    //candidates 用 nums 来代替
    void findCombinationSum(vector<int>& nums, int target, int len_mod, vector<int> &temp,
    vector<vector<int>> &res, int loc){
        //测试代码
        /*
        cout << "target:  " << target << "    ";
        cout << "len_mod:  " << len_mod << "   ";
        cout << "temp.szie  " << temp.size() << "  ";
        cout << endl
        */
        //边界条件
        if( target == 0){
            //查找成功。
            //加入数组
            //清空暂存
            res.push_back(temp);
            //清楚末尾，执行下一次查找
            temp.pop_back();
            //temp.erase(temp.end()); //为什么会越界。
            return ;
        }else if( target < nums[0]){
            //小于最小值，返回
            temp.pop_back();
            //temp.erase(temp.end());
            return ;
        }
        //开始查找
        //先重新判断数组长度，减少冗余计算
        int len_remod = len_mod;
        for(int i = len_mod - 1; i >= 0; i--){
            if( nums[i] <= target){
                len_remod = i + 1;
                break;
            }           
        }
        //防止重复查找。
        len_remod = len_remod > (loc + 1) ? (loc + 1) : len_remod;
        //递归查找
        int value;
        for(int i = len_remod - 1; i >= 0; i--){
            //末尾元素先加入，如果查找失败了会删除。
            temp.push_back(nums[i]);
            value = target - nums[i];
            //测试的代码
            //cout << "  nums[i] " << nums[i] << endl;
            findCombinationSum(nums, value, len_remod, temp, res, i);
            //即使查找成功了，剩余的数依旧要去试一试。
        }
        temp.pop_back();
        return ;
    };                                                                                  
public:

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        //获取长度
        int len = candidates.size();
        //返回结果。
        vector<int> temp;
        vector<vector<int>> res;
        //判断特殊情况
        /*
        if( len == 1 ){
            if( candidates[0] == target ){
                temp.push_back(candidates[0]);
                res.push_back(temp);
            }
            return res;
        }
        */
        //排序， 1 <= len <= 30; 复杂度为常数级别。
        //默认从小到大。而且题意中已经说明没有重复元素。
        sort(candidates.begin(), candidates.end());
        //排序之后利于遍历。
        //小于最小值。直接返回
        if( target < candidates[0]){
            return res;
        }
        //排除掉大于target的那些值
        int len_mod = 0;
        //不可能都大于，否则前面已经返回了。
        for( int i = len - 1; i >= 0; i--){
            if( candidates[i] <= target){
                len_mod = i + 1;
                break;
            }
        }
        //已经排除了所有大于target的元素
        //递归查找。
        findCombinationSum(candidates, target, len_mod, temp, res, len_mod);
        //返回最终结果
        return res;
    }
};
```

### 知识点：

最为关键的就是递归实现的思路。

此外还有很多的细节需要注意。

​		递归函数有无必要返回？

​		边界条件应该是什么？

​		剪枝如何实现？

​		递归传递的参数有哪些？

### 官方代码：

```
class Solution {
public:
    void dfs(vector<int>& candidates, int target, vector<vector<int>>& ans, vector<int>& combine, int idx) {
        if (idx == candidates.size()) {
            return;
        }
        if (target == 0) {
            ans.emplace_back(combine);
            return;
        }
        // 直接跳过
        dfs(candidates, target, ans, combine, idx + 1);
        // 选择当前数
        if (target - candidates[idx] >= 0) {
            combine.emplace_back(candidates[idx]);
            dfs(candidates, target - candidates[idx], ans, combine, idx);
            combine.pop_back();
        }
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> ans;
        vector<int> combine;
        dfs(candidates, target, ans, combine, 0);
        return ans;
    }
};
```

### 热评：

```
看的题越多，越感觉自己和傻子没什么区别。
```

