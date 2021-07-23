### 题目：

给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

### 难度：

中等。

### 示例：

```
输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。
```

### 解题思路：

```
这道题与上道题思路基本一样。
直接修改上道题的代码。
将判断是否为零改为值与target的差值。
1、首先判断特殊情况，如果nums的长度为3，直接返回三数之和。
2.排序后遍历：
    尽量避免相同数据的重复计算，如果和恰好为target，直接返回。
```

### 代码实现：

```c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        //返回结果为int类型的值。
        //sum存储最接近的和。temp暂存临时值.
        int sum, temp;
        //数组长度和左右指针。
        int len = nums.size();
        int left, right;
        //先给sum赋一个值，以防出现target = 0的情形。
        sum = nums[0] + nums[1] + nums[2];
        //判断特殊情况。长度为3，直接返回三数之和。
        if( len == 3){
            return sum;
        }       
        //对向量进行排序。利用了C++中的sort函数。
        sort(nums.begin(), nums.end());
        //求解
        //从第一个数查到倒数第三个数。
        for(int i=0; i < len - 2; ){
            //定位left, right.
            left = i + 1;
            right = len - 1;
            //边界条件
            while( left < right ){
                temp = nums[i] + nums[left] + nums[right];
                //cout << "temp:" << temp << "sum:" << sum << endl;  
                //temp的绝对值更小.记录这一组数据。left，right移位
                if( abs(temp - target) <= abs(sum - target)){
                    sum = temp;
                }
                if(temp == target){
                    return sum;
                }
                //小于target，left太小了，右移至下一个不相同的数。  
                else if( temp < target ){
                    left++;
                    while( (left <= right) && (nums[left] == nums[left - 1]) ){
                        left++;
                    }
                }
                //大于target，right太大了，左移至下一个不相同的数。 
                else if( temp > target ){
                    right--;
                    while( (right >= left) && (nums[right] == nums[right + 1])){
                        right--;
                    }
                }
            }
            i++;
            //先判断当前数字与前面的数字是否一样
            //一样则跳过。否则会造成重复解。
            while((i < len - 2) && (nums[i] == nums[i-1])){
                i++;
            }
        }
        return sum;
    }
};
```

### 知识点：

依旧是双指针的使用，与暴力遍历不同的是，尽可能避免无意义的计算。虽然引入了一次排序，但是避免了更多没有必要的求和。

### 官方代码：

```
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        int best = 1e7;

        // 根据差值的绝对值来更新答案
        auto update = [&](int cur) {
            if (abs(cur - target) < abs(best - target)) {
                best = cur;
            }
        };

        // 枚举 a
        for (int i = 0; i < n; ++i) {
            // 保证和上一次枚举的元素不相等
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            // 使用双指针枚举 b 和 c
            int j = i + 1, k = n - 1;
            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];
                // 如果和为 target 直接返回答案
                if (sum == target) {
                    return target;
                }
                update(sum);
                if (sum > target) {
                    // 如果和大于 target，移动 c 对应的指针
                    int k0 = k - 1;
                    // 移动到下一个不相等的元素
                    while (j < k0 && nums[k0] == nums[k]) {
                        --k0;
                    }
                    k = k0;
                } else {
                    // 如果和小于 target，移动 b 对应的指针
                    int j0 = j + 1;
                    // 移动到下一个不相等的元素
                    while (j0 < k && nums[j0] == nums[j]) {
                        ++j0;
                    }
                    j = j0;
                }
            }
        }
        return best;
    }
};

```

### 热评：

```
其实c++ 暴力解法 (O(N^3))并没有超出时间限制 C++ 它站起来了！！！
```

```
其实python也。。。。。啊他没站起来。
```

```
我又觉得我行了（滑稽）
```

