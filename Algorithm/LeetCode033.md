### 题目：

整数数组 nums 按升序排列，数组中的值 互不相同 。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为 [4,5,6,7,0,1,2] 。

给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回 -1 。

### 难度：

中等。

### 示例：

```
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

### 解题思路：

```
首先观察这个数组，分为两个部分，都为升序排列。后面一部分整体小于前半部分。
所以可以先进行一次判断。与nums[0]和nums[size - 1]比较，看到底属于哪一部分。
然后可以利用二分查找进行查找。
对不同的情况进行讨论。
flag = 0；
    二分法，如果中间的数小于 nums[0],继续向左二分。如果大于nums[0]，与target比较，考虑下一次的转向。
```

### 代码实现：

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        //考虑特殊情况。
        int len = nums.size();
        //设置标志，判断可能属于哪一序列。
        int flag; //0代表前一序列，1代表后一序列。
        if( len == 1 ){
            return nums[0] == target ? 0 : -1;
        }
        if( target == nums[0]){
            return 0;
        }else if( target == nums[len - 1] ){
            return len - 1;
        }
        if((target > nums[len - 1]) && (target < nums[0])){
            return -1;
        }else if( target < nums[ len -1]){
            //可能在后一序列。
            flag = 1;
        }else if( target > nums[0]){
            //可能在前一序列。
            flag = 0;
        }   
        //接下来就是二分法
        int head = 0;
        int toil = len - 1;
        int middle;
        if(flag == 0){
            while(head <= toil){
                cout << "head = " << head << "  " << "toil = " << toil << endl;
                middle = (head + toil) / 2;
                if( nums[middle] == target){
                    return middle;
                }else if( nums[middle] < nums[head] || target < nums[middle]){
                    toil = middle - 1;
                }else if( target > nums[middle] ){  //nums[middle] > nums[head];
                    head = middle + 1;
                }
            }
        }else if(flag == 1){
            while(head <= toil){
                cout << "head = " << head << "  " << "toil = " << toil << endl;
                middle = (head + toil) / 2;
                if( nums[middle] == target){
                    return middle;
                }else if( nums[middle] > nums[toil] || target > nums[middle]){
                    head = middle + 1;
                }else if( target < nums[middle] ){  //nums[middle] > nums[head];
                    toil = middle - 1;
                }
            }

        }
        return -1;
    }
};
```

### 知识点：

跟官方代码一比，思路一致，但很多细节没有考虑到位，代码也比较冗余。还有很多可以改进的点。

### 官方代码：

```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = (int)nums.size();
        if (!n) {
            return -1;
        }
        if (n == 1) {
            return nums[0] == target ? 0 : -1;
        }
        int l = 0, r = n - 1;
        while (l <= r) {
            int mid = (l + r) / 2;
            if (nums[mid] == target) return mid;
            if (nums[0] <= nums[mid]) {
                if (nums[0] <= target && target < nums[mid]) {
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
        return -1;
    }
};
```

### 热评：

```
想到了，但是实现有问题，还是忍不住来看答案。
```

```
有序就想到二分！不过细节就是问题了。手是我的，脑子不这么想。脑子说它是人体最聪明的器官，可是这是脑子告诉你的。
```

