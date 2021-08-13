### 题目：

给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 最多出现两次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

说明：

为什么返回数值是整数，但输出的答案是数组呢？

请注意，输入数组是以「引用」方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

### 难度：

中等。

### 示例：

```
输入：nums = [1,1,1,2,2,3]
输出：5, nums = [1,1,2,2,3]
解释：函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3 。 不需要考虑数组中超出新长度后面的元素
```

### 解题思路：

```
    数组有序：
        使用两个指针和一个暂存数字，同时记录当前数字的出现次数。
        一个指针遍历数组，一个指针指向合法字符的最后一位。
```

### 代码实现：

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        //获取nums的长度
        int len = nums.size();
        //定义变量
        int temp,j;
        int count;
        //遍历数组
        j=0;
        count = 1;
        for(int i=1; i<len; i++){
            if(nums[i] != nums[j]){
                j++;
                nums[j] = nums[i];
                count = 1;
            }else if(nums[i] == nums[j] && count == 1){
                j++;
                nums[j] = nums[i];
                count++;
            }
        }

        return ++j;
    }
};
```

### 知识点：

与之前题目的区别只是 最大长度变成2了而已。

### 官方代码：

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if (n <= 2) {
            return n;
        }
        int slow = 2, fast = 2;
        while (fast < n) {
            if (nums[slow - 2] != nums[fast]) {
                nums[slow] = nums[fast];
                ++slow;
            }
            ++fast;
        }
        return slow;
    }
};
```

### 热评：

```
我的双指针什么时候能这么优雅 /(ㄒoㄒ)/~~
```

