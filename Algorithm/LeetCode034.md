### 题目：

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 [-1, -1]。

进阶：

你可以设计并实现时间复杂度为 O(log n) 的算法解决此问题吗？

### 难度：

中等。

### 示例：

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

### 解题思路：

```
同样也采用二分法。但不同的是需要找到同一元素的第一个和最后一个元素。
如何找到最左面的元素。即使使用二分法查找到了，也需要继续往左查找。
然后以最左元素为起点，开始使用二分法查找最右元素。
思路：
    先查找最左值：
        如果nums[middle] == target , 记录当前middle，继续向左遍历
        如果nums[middle] <= target , 向右遍历。
        如果nums[middle] >= target , 向左遍历，同时记录右边界，以减少没有必要的重复遍历。
    判断是否查找到了左值：
        如果没有，直接返回。
        如果有，说明至少存在一个target。
    查找最右值：
        左边界为最左值，而且数组不减，所有不可能小于target。
        右边界为刚才存储的toilRight。之后进行二分遍历
        如果nums[middle] == target , 记录当前middle，继续向右遍历  
        如果nums[middle] >= target , 向左遍历。  
```

### 代码实现：

```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        //获取数组长度。
        int len = nums.size();
        //左右位置
        int left = -1;
        int right = -1;
        //返回结果
        vector<int> res;
        res.push_back(left);
        res.push_back(right);
        //讨论特殊情况
        if( len == 0){
            return res;
        }
        if( target < nums[0] && target > nums[len - 1]){
            return res;
        }
        //开始查找最左元素。
        int head = 0;
        int toil = len - 1;
        int middle;
        //用来记录寻找右元素时的尾部。
        //减少遍历长度
        int toilRight = toil;
        while(head <= toil){
            middle = (head + toil) / 2;
            if(nums[middle] == target){
                //先记录下当前位置
                left = middle;
                //向左遍历，找最左边那个
                toil = middle - 1;  
            }else if( nums[middle] < target){
                //向右查找
                head = middle + 1;
            }else if( nums[middle] > target){
                toil = middle - 1;
                toilRight = toil;
                
            }
        }
        cout << toilRight << endl;
        cout << left << endl;
        //判断target是否存在
        if( left == -1){
            return res;
        }
        res[0] = left;
        //查找最右元素位置
        head = left;
        toil = toilRight;
        while( head <= toil ){
            middle = (head + toil) / 2;
            //nums[middle] 不可能小于 target。
            if(nums[middle] == target){
                //先记录下当前位置
                right = middle;
                //向右遍历，找最右边那个
                head = middle + 1;  
            }else if( nums[middle] > target){
                toil = middle - 1;
            }            
        }
        res[1] = right;
        return res; 
    }
};
```

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        left = 0
        right = len(nums)
        while (left < right):
            mid = (left + right) // 2
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid
            else:
                # 这里变成了暴力破解法
                head = mid
                while head - 1 >= 0 and nums[head - 1] == target:
                    head -= 1
                tail = mid
                while tail + 1 < len(nums) and nums[tail + 1] == target:
                    tail += 1
                
                return [head, tail]
        # 查找失败
        return [-1, -1]
```

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        left = 0
        right = len(nums)
        while (left < right):
            mid = (left + right) // 2
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid
            else:
                # 使用不同的函数
                head = self.searchHead(nums, target)
                tail = self.searchTail(nums, target)               
                return [head, tail]

        # 查找失败
        return [-1, -1]       
    
    def searchHead(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums)
        head = -1
        while (left < right):
            mid = (left + right) // 2
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid
            else:
                head = mid
                right = mid

        return  head     

    def searchTail(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums)
        Tail = -1
        while (left < right):
            mid = (left + right) // 2
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid
            else:
                Tail = mid
                left = mid + 1

        return  Tail
```

```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size();
        vector<int> ret = {-1, -1};
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] == target) {
                ret[0] = binarySearch(nums, target, true);
                ret[1] = binarySearch(nums, target, false) - 1;
                return ret;
            } else if (nums[mid] > target) {
                right = mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            }
        }
        // 查找失败
        return ret;
    }
    
    int binarySearch(vector<int>& nums, int target, bool flag) {
        int left = 0, right = nums.size();
        int res = nums.size();
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            // 判断关键
            if (nums[mid] > target || (flag && nums[mid] >= target)) {
                right = mid;
                res = mid;
            } else {
                left = mid + 1;
            }
        }
        return res;
    }
};
```

### 知识点：

官方代码依旧简洁.jpg。但起码自己还是独立完成了这道题。下次接着加油。

### 官方代码：

```
class Solution { 
public:
    int binarySearch(vector<int>& nums, int target, bool lower) {
        int left = 0, right = (int)nums.size() - 1, ans = (int)nums.size();
        while (left <= right) {
            int mid = (left + right) / 2;
            if (nums[mid] > target || (lower && nums[mid] >= target)) {
                right = mid - 1;
                ans = mid;
            } else {
                left = mid + 1;
            }
        }
        return ans;
    }

    vector<int> searchRange(vector<int>& nums, int target) {
        int leftIdx = binarySearch(nums, target, true);
        int rightIdx = binarySearch(nums, target, false) - 1;
        if (leftIdx <= rightIdx && rightIdx < nums.size() && nums[leftIdx] == target && nums[rightIdx] == target) {
            return vector<int>{leftIdx, rightIdx};
        } 
        return vector<int>{-1, -1};
    }
};
```

### 热评：

```
什么才叫不讲码德，API选手参见！

/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var searchRange = function(nums, target) {
    let left = nums.indexOf(target)
    let right = nums.lastIndexOf(target)
    return [left,right]
};
```

