### 题目：

实现获取 下一个排列 的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须 **原地** 修改，只允许使用额外常数空间。

### 难度：

中等。

### 示例：

```
输入：nums = [3,2,1]
输出：[1,2,3]
输入：nums = [1,2,3]
输出：[1,3,2]
```

### 解题思路：

```
从后往前比较，设后k个数为降序排列。判断后k+1个数是否为降序排列。
    如果是，向前移动
    如果不是，找到前k个数种大于第K+1个数的最小的数。与K+1互换剩余k个数升序排列
遍历结束说明整个数组降序排列。返回数组升序排列。
```

### 代码实现：

```c++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        //排除特殊情形
        if( nums.size() < 2 ){
            return ;
        }
        //获取长度
        int len = nums.size();
        //创建指针
        int p;
        //计数
        int count=0;
        //k指向倒数第k个元素。
        for(int k = len - 1; k > 0 ; k--){
            //p指向倒数k+1个。
            p = k - 1;
            count++;
            //判断第k+1个是否大于第K个
            //如果是进行下一次循环
            //如果不是就要对后K+1个数进行排列。
            if( nums[p] < nums[k]){
                //这时后count个数为降序排列
                for( int i = len -1; i > p; i--){
                    if( nums[p] < nums[i]){
                        //找到大于第K+1个数的最小数
                        swap(nums[p], nums[i]);
                        //剩余k个数升序排列
                        sort(nums.end() - count,nums.end());
                        return ;
                    }
                }
            }
        }
        sort(nums.begin(), nums.end());
        return ;
    }
};
```

### 知识点：

比较有意思的是这个题目描述和思路。考虑什么情况下才是符合题意的排列。

### 官方代码：

```
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int i = nums.size() - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            i--;
        }
        if (i >= 0) {
            int j = nums.size() - 1;
            while (j >= 0 && nums[i] >= nums[j]) {
                j--;
            }
            swap(nums[i], nums[j]);
        }
        reverse(nums.begin() + i + 1, nums.end());
    }
};
```

### 热评：

```
这个题每个字我都认识，连起来就不知道了
```

```
能大，只能大一点点。
```

