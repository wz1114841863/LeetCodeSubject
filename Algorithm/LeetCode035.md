### 题目：

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 O(log n) 的算法。

### 难度：

简单。

### 示例：

```
输入: nums = [1,3,5,6], target = 5
输出: 2
输入: nums = [1,3,5,6], target = 7
输出: 4
```

### 解题思路：

```
依旧是二分法。唯一区别在于返回值。
```

### 代码实现：

```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        //获取数组长度
        int len = nums.size();
        //判断特殊情况
        if( len == 0 ){
            return 0;
        }
        //返回结果
        //二分法
        int head = 0;
        int toil = len - 1;
        int middle;
        while( head <= toil){
            middle = (head + toil) / 2;
            if( nums[middle] == target){
                return middle;
            }else if( nums[middle] < target){
                head = middle + 1;
            }else if( nums[middle] > target){
                toil = middle - 1;
            }
        }
        //cout << head << endl;
        //cout << toil << endl;
        return head;
    }
};
```

```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size();
        while (left < right){
            int middle = left + ((right - left) >> 1);
            if (nums[middle] == target) {
                return middle;
            }
            else if (nums[middle] < target) {
                left = middle + 1;
            }
            else if (nums[middle] > target) {
                right = middle;
            }
        }
        // 查找失败
        return right;
    }
};
```

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums)
        while (left < right):
            middle = (left + right) // 2
            if nums[middle] == target:
                return middle
            elif nums[middle] < target:
                left = middle + 1
            elif nums[middle] > target:
                right = middle
        # 查找失败
        return right
```

### 知识点：

最后的返回值其实我是看返回的head和toil值选出来的。[doge]

### 官方代码：

```
class Solution {
    public int searchInsert(int[] nums, int target) {
        int n = nums.length;
        int left = 0, right = n - 1, ans = n;
        while (left <= right) {
            int mid = ((right - left) >> 1) + left;
            if (target <= nums[mid]) {
                ans = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return ans;
    }
}
```

### 热评：

```
是看到星期五，我要看乘风破浪的姐姐，为了让我每日题量达标所以选择这么简单的题吗？爱了爱了！！
```

