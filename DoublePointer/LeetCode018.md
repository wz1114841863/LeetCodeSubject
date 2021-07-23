### 题目：

给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。

注意：答案中不可以包含重复的四元组

### 难度：

中等。

### 示例：

```
输入：nums = [1,0,-1,0,-2,2], target = 0
输出：[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

### 解题思路：

```
初始想法：
之前已经做了三数之和，能不能将第四个数定在最右边，不断向内缩进。
```

### 代码实现：

```c++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        //返回结果为vector<vector<int>>类型的值。
        vector<vector<int>> result;
        //第一步判断向量长度是否大于4
        if( nums.size() < 4 ){
            return result;
        }
        //对向量进行排序。利用了C++中的sort函数。
        sort(nums.begin(), nums.end());
        //获取长度，定义和
        int len = nums.size();   
        int  sum;  
        //特殊情况。
        if((nums[0] > 0) && (target <= 0)){
            return result;
        }else if(( nums[ len - 1 ] < 0) && (target >= 0)){
            return result;
        }   
        if( len == 4){

            sum = nums[0] + nums[1] + nums[2] + nums[3];
            if(sum == target ){
                result.push_back(nums);    
            }
            return result;
        }
        //判断是否可能有解
        int left, right;
        vector<int> temp;
        //可能有解即遍历
        //i做第一个数的指针，从第一个数查到倒数第四个数。
        for(int i=0; i < len - 3; ){
            //j做第二个数的指针，从最后一个数一直到 i+3
            for(int j=len - 1; j > i + 2; ){
                //定位left, right.
                left = i + 1;
                right = j - 1;
                //边界条件
                while( left < right ){

                    sum = nums[i] + nums[left] + nums[right] + nums[j];
                    //等于0.记录这一组数据。left，right移位
                    if(sum == target){
                        temp.clear();
                        temp.push_back(nums[i]);
                        temp.push_back(nums[left]);
                        temp.push_back(nums[right]);
                        temp.push_back(nums[j]);
                        result.push_back(temp);
                        left++;
                        while((left <= right) && (nums[left] == nums[left - 1]) ){
                            left++;
                        }
                        right--;
                        while( (right >= left) && (nums[right] == nums[right + 1]) ){
                            right--;
                        }

                    }
                    //小于0，left太小了，右移至下一个不相同的数。  
                    else if( sum < target ){
                        left++;
                        while( (left <= right) && (nums[left] == nums[left - 1]) ){
                            left++;
                        }
                    }
                    else if( sum > target ){
                        right--;
                        while( (right >= left) && (nums[right] == nums[right + 1])){
                            right--;
                        }
                    }
                }
                j--;
                //先判断当前数字与后面的数字是否一样
                //一样则跳过。否则会造成重复解。
                while((j > i + 2) && (nums[j] == nums[j+1])){
                    j--;
                }
            }

            i++;
            //先判断当前数字与前面的数字是否一样
            //一样则跳过。否则会造成重复解。
            while((i < len - 3) && (nums[i] == nums[i-1])){
                i++;
            }
        }
        return result;
    }
};
```

### 知识点：

最奇怪的测试用例是：

[1000000000, 1000000000, 1000000000, 1000000000]
0

### 官方代码：

```
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> quadruplets;
        if (nums.size() < 4) {
            return quadruplets;
        }
        sort(nums.begin(), nums.end());
        int length = nums.size();
        for (int i = 0; i < length - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            if (nums[i] + nums[i + 1] + nums[i + 2] + nums[i + 3] > target) {
                break;
            }
            if (nums[i] + nums[length - 3] + nums[length - 2] + nums[length - 1] < target) {
                continue;
            }
            for (int j = i + 1; j < length - 2; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }
                if (nums[i] + nums[j] + nums[j + 1] + nums[j + 2] > target) {
                    break;
                }
                if (nums[i] + nums[j] + nums[length - 2] + nums[length - 1] < target) {
                    continue;
                }
                int left = j + 1, right = length - 1;
                while (left < right) {
                    int sum = nums[i] + nums[j] + nums[left] + nums[right];
                    if (sum == target) {
                        quadruplets.push_back({nums[i], nums[j], nums[left], nums[right]});
                        while (left < right && nums[left] == nums[left + 1]) {
                            left++;
                        }
                        left++;
                        while (left < right && nums[right] == nums[right - 1]) {
                            right--;
                        }
                        right--;
                    } else if (sum < target) {
                        left++;
                    } else {
                        right--;
                    }
                }
            }
        }
        return quadruplets;
    }
};

```

### 热评：

```
高端的coder往往用最朴素的解法。
```

