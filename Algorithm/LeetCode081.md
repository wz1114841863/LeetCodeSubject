### 题目：

已知存在一个按非降序排列的整数数组 nums ，数组中的值不必互不相同。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转 ，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,4,4,5,6,6,7] 在下标 5 处经旋转后可能变为 [4,5,6,6,7,0,1,2,4,4] 。

给你 旋转后 的数组 nums 和一个整数 target ，请你编写一个函数来判断给定的目标值是否存在于数组中。如果 nums 中存在这个目标值 target ，则返回 true ，否则返回 false 。

### 难度：

中等。

### 示例：

```
输入：nums = [2,5,6,0,0,1,2], target = 0
输出：true
```

### 解题思路：

```
    数组非降序,整体分为两个部分，皆为升序。
    与之前题比较，可能出现重复数字而无法二分的情况，此时，left++， right--。
```

### 代码实现：

```c++
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        //获取数组长度
        int len = nums.size();
        //处理特殊情况
        if(!len){
            return false;
        }
        if(len == 1){
            return nums[0] == target;
        }
        //二分遍历
        int left = 0, right = len - 1, middle;
        while (left <= right) {
            middle = (left + right) / 2;
            //cout << "left  " << left << "right  "  << right << "middle  " << middle  << endl;
            if (nums[middle] == target){
                return true;
            } 
            //特殊情况
            if(nums[left] == nums[middle] && nums[left] == nums[right]){
                left++;
                right--;
            }else if (nums[left] <= nums[middle]) {
                if (nums[left] <= target && target < nums[middle]) {
                    right = middle - 1;
                } else {
                    left = middle + 1;
                }
            } else {
                if (nums[middle] < target && target <= nums[len - 1]) {
                    left = middle + 1;
                } else {
                    right= middle - 1;
                }
            }

        }
        return false;
    }
};
```

### 知识点：

由于是改的之前题目的官方代码，所以这次代码基本一致，只是增加了一次判断情况。

```
if(nums[left] == nums[middle] && nums[left] == nums[right]){
	left++；
	right--;
}
```

### 官方代码：

```
class Solution {
public:
    bool search(vector<int> &nums, int target) {
        int n = nums.size();
        if (n == 0) {
            return false;
        }
        if (n == 1) {
            return nums[0] == target;
        }
        int l = 0, r = n - 1;
        while (l <= r) {
            int mid = (l + r) / 2;
            if (nums[mid] == target) {
                return true;
            }
            if (nums[l] == nums[mid] && nums[mid] == nums[r]) {
                ++l;
                --r;
            } else if (nums[l] <= nums[mid]) {
                if (nums[l] <= target && target < nums[mid]) {
                    r = mid - 1;
                } else {
                    l = mid + 1;
                }
            } else {
                if (nums[mid] < target && target <= nums[n - 1]) {
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
            }
        }
        return false;
    }
};

```

### 热评：

```
#击败72.19%
return target in nums
```

