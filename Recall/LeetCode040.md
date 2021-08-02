### 题目：

给定一个数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用一次。

注意：解集不能包含重复的组合。 

### 难度：

中等。

### 示例：

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
输出:
[ [1,1,6], [1,2,5], [1,7], [2,6]]
 
```

### 解题思路：

```
修改上一题的代码。关键在于去重.
去重的思路有两个：
    1.外层使用for循环遍历
    2.内层排除刚想要入栈的元素是否与刚弹出来的元素相同。
尝试改进，去掉for循环失败。
```

### 代码实现：

```c++
class Solution {
private:
    //判断是否与前一出栈数字相同
    //如果是，跳过该数字。
    int temp_pre = 0;
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
        */
        cout << endl;
        //边界条件
        if( target == 0){
            //查找成功。
            //加入数组
            //清空暂存
            res.push_back(temp);
            //清楚末尾，执行下一次查找
            temp_pre = temp.back();
            temp.pop_back();
            //temp.erase(temp.end()); //为什么会越界。
            return ;
        }else if( target < nums[0] ){
            //小于最小值，返回
            temp_pre = temp.back();
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
        /*

        */
        for(int i = len_remod - 1; i >= 0; i--){
            if( nums[i] == temp_pre ){
                continue;
            }
            //末尾元素先加入，如果查找失败了会删除。
            temp.push_back(nums[i]);
            value = target - nums[i];
            //测试的代码
            //cout << "  nums[i] " << nums[i] << endl;
            findCombinationSum(nums, value, len_remod - 1, temp, res, i - 1);
            //即使查找成功了，剩余的数依旧要去试一试。
        }
        temp_pre = temp.back();
        temp.pop_back();
        return ;
    };                                                                                  
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        //获取长度
        int len = candidates.size();
        //返回结果。
        vector<int> temp;
        vector<vector<int>> res;
        //排序， 1 <= len <= 100; 复杂度为常数级别。
        //默认从小到大。
        sort(candidates.begin(), candidates.end());
        //排序之后利于遍历。
        //小于最小值。直接返回
        if( target < candidates[0]){
            return res;
        }
        //判断特殊情况
        if( len == 1){
            temp.push_back(candidates[0]);
            if( candidates[0] == target){
                res.push_back(temp);
            }
            return res;
        }
        //排除掉大于target的那些值
        int len_mod = len;
        //不可能都大于，否则前面已经返回了。
        for( int i = len - 1; i >= 0; i--){
            if( candidates[i] <= target){
                len_mod = i + 1;
                break;
            }
        }
        //已经排除了所有大于target的元素
        //递归查找。这里要考虑去重。
        for( int i = len_mod - 1; i >= 0; i--){

            temp.push_back(candidates[i]);
            findCombinationSum(candidates, target - candidates[i], len_mod - 1, temp, res, i - 1);
            //去重
            while((i != 0) && (candidates[i] == candidates[i-1])){
                i--;
            }
        }
        //是否可以去掉上面这项for循环。
        //findCombinationSum(candidates, target, len_mod - 1, temp, res, len_mod - 1);
        //返回最终结果
        return res;
    }
};
```

### 知识点：

关键点就在于如何排除重复的答案，但又不会排除[6, 1, 1 ]这样的答案。

递归的想法对我来说还是想思考很久。哎。

### 官方代码：

```
class Solution {
private:
    vector<pair<int, int>> freq;
    vector<vector<int>> ans;
    vector<int> sequence;

public:
    void dfs(int pos, int rest) {
        if (rest == 0) {
            ans.push_back(sequence);
            return;
        }
        if (pos == freq.size() || rest < freq[pos].first) {
            return;
        }

        dfs(pos + 1, rest);

        int most = min(rest / freq[pos].first, freq[pos].second);
        for (int i = 1; i <= most; ++i) {
            sequence.push_back(freq[pos].first);
            dfs(pos + 1, rest - i * freq[pos].first);
        }
        for (int i = 1; i <= most; ++i) {
            sequence.pop_back();
        }
    }

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        for (int num: candidates) {
            if (freq.empty() || num != freq.back().first) {
                freq.emplace_back(num, 1);
            } else {
                ++freq.back().second;
            }
        }
        dfs(0, target);
        return ans;
    }
};
```

### 热评：

```
看了俩小时答案,我不是人.
```

