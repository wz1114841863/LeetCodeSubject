### 题目：

给定一个不含重复数字的数组 `nums` ，返回其 **所有可能的全排列** 。你可以 **按任意顺序** 返回答案。



### 难度：

中等。

### 示例：

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

### 解题思路：

```
利用递归，循环调用，要想先排列n个先排列n-1个。
    边界条件：
        if( n == 2) {
            两个数按序插入后，将向量插入到res；
            交换两个数的顺序，再次插入，将数组插入到res;
        }
        if( n > 2){
            for( 遍历每一个元素){
                排序其余 n - 1个元素。
            }
        }

```

### 代码实现：

```c++
class Solution {
private:
    void arrange(vector<vector<int>> &res, const vector<int> &nums, vector<int> &temp){
        //获取nums的长度
        int len1 = nums.size();
        if( len1 == 2){
            temp.push_back(nums[0]);  temp.push_back(nums[1]);
            res.push_back(temp);
            temp.pop_back();  temp.pop_back();
            temp.push_back(nums[1]);  temp.push_back(nums[0]);
            res.push_back(temp);
            temp.pop_back();  temp.pop_back();
        }
        // n > 2, 调用 n-1
        //如何传递 n - 1 这样以一个数组
        vector<int> tempNums;
        for( int i = 0; i <= len1 - 1; i++){
            tempNums = nums;
            temp.push_back(nums[i]);
            tempNums.erase( tempNums.begin()+i);
            arrange(res, tempNums, temp);
            temp.pop_back();
        }
    }    
public:
    vector<vector<int>> permute(vector<int>& nums) {
        //获取数组长度
        int len =  nums.size();
        //最终返回结果
        vector<vector<int>> res;
        //判断特殊情况
        if( len == 1 ){
            res.push_back(nums);
            return res;
        }
        //数组暂存
        vector<int> temp;
        //调用递归函数，传递res 和 nums
        arrange(res, nums, temp);
        
        return res;
    }
};
```

### 知识点：

递归类型的题可以给位非递归来提高效率。但是代码的复杂度会提升。

### 官方代码：

```
class Solution {
public:
    void backtrack(vector<vector<int>>& res, vector<int>& output, int first, int len){
        // 所有数都填完了
        if (first == len) {
            res.emplace_back(output);
            return;
        }
        for (int i = first; i < len; ++i) {
            // 动态维护数组
            swap(output[i], output[first]);
            // 继续递归填下一个数
            backtrack(res, output, first + 1, len);
            // 撤销操作
            swap(output[i], output[first]);
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int> > res;
        backtrack(res, nums, 0, (int)nums.size());
        return res;
    }
};
```

### 热评：

```
我一直以为这种方法叫深度搜索。
```

