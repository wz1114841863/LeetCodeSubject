### 题目：

给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

说明:

为什么返回数值是整数，但输出的答案是数组呢?

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

简单。

### 示例：

```
输入：nums = [1,1,2]
输出：2, nums = [1,2]
解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素
```

### 解题思路：

```
注意是有序链表。所以更容易实现。
对比当前数与上一个数：
    如果不相等，保留。
    如果相等，舍弃。
双指针：
    一个指针遍历数组。
    一个指针指向不重复数字的最后一位。
```

### 代码实现：

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        //排除特殊情况。
        if( nums.size() < 2){
            return nums.size();
        }
        //遍历数组
        int j=0;
        for( int i=1; i < nums.size(); i++){
            if( nums[i] != nums[j]){
                j++;
                nums[j] = nums[i];
            }
        }
        return ++j;
    }
};
```

### 知识点：

注意到是有序数组。只有可能与前面最近的数相等。

### 官方代码：

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return 0;
        }
        int fast = 1, slow = 1;
        while (fast < n) {
            if (nums[fast] != nums[fast - 1]) {
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
一年前的我居然写出来了。
```

