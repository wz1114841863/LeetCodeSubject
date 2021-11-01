### 题目：

```
符合下列属性的数组 arr 称为 山脉数组 ：
	arr.length >= 3
	存在 i（0 < i < arr.length - 1）使得：
		arr[0] < arr[1] < ... arr[i-1] < arr[i]
		arr[i] > arr[i+1] > ... > arr[arr.length - 1]

给你由整数组成的山脉数组 arr ，返回任何满足 arr[0] < arr[1] < ... arr[i - 1] < arr[i] > arr[i + 1] > ... > arr[arr.length - 1] 的下标 i 。
```

### 难度：

简单。

### 示例：

```
输入：arr = [24,69,100,99,79,78,67,36,26,19]
输出：2
```

### 解题思路：

```c++
    解题思路：
    O(n)时间复杂度，遍历即可，数组元素大小先升后降。
    O(logn)时间复杂度，二分查找变形。
    三种情况：
        1.mid >= low, mid < high
            最大值一定位于（mid, high]之间。
        2.mid <= low , mid > high
            最大值一定位于 [low，mid)之间。
        3.mid > low, mid > high
            最大值在(low, high)之间
            将区间分为[low+1, midd-1], [middle, high - 1], 递归
    改进：
        不利用 arr[mid] 与 arr[low]、arr[high]去比
        而是利用arr[mid] 与 arr[mid - 1]、arr[mid + 1]去比。
    官方解答：
        仅使用arr[mid] 与 arr[mid + 1]去比。
```

### 代码实现：

```c++
 /*
class Solution {
public:
    void bsearch(vector<int>& arr,int low,int high){
        if(low > high) 
            return; //终止条件
        if(low == high){
            if(arr[high] > max){
                max = arr[high];
                loc = high;
            }
            return ;            
        }
        int mid = low + ((high - low) >> 1);
        if(arr[mid] >= arr[low] && arr[mid] < arr[high]){
            bsearch(arr, mid + 1, high);
        }else if(arr[mid] <= arr[low] && arr[mid] > arr[high]){
            bsearch(arr, low, mid);
        }else if(arr[mid] > arr[low] && arr[mid] > arr[high]){
            bsearch(arr, low + 1,  mid - 1);
            bsearch(arr, mid, high);
        }

        return ;
    }
    int peakIndexInMountainArray(vector<int>& arr) {
        loc = -1;
        bsearch(arr, 0, arr.size() - 1);
        return loc;
    }
private:
    int max;
    int loc;
};
*/
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& arr) {
        int low = 0;
        int high = arr.size() - 1;
        int mid = 0;
        while(low < high){ 
            mid = low + ((high - low) >> 1);
            if(arr[mid] > arr[mid + 1] && arr[mid] > arr[mid - 1]){
                break;//结束，找到山峰。
            }else if(arr[mid] < arr[mid + 1] && arr[mid] > arr[mid - 1]){
                low = mid;
            }else if(arr[mid] > arr[mid + 1] && arr[mid] < arr[mid - 1]){
                high = mid;
            }
        }
        return mid;
    }

};
```

### 知识点：

即使是很简单的一道题也反映出思路的重要性。

可以看出官方算法的简洁性。

### 官方代码：

```c++
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& arr) {
        int n = arr.size();
        int left = 1, right = n - 2, ans = 0;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (arr[mid] > arr[mid + 1]) {
                ans = mid;
                right = mid - 1;
            }
            else {
                left = mid + 1;
            }
        }
        return ans;
    }
};
```

### 热评：

```
二分的while循环判断条件什么时候是＜什么时候是≤呀？有没有什么技巧？难道每个类型的二分都要自己debug一下来确定吗
个人理解，如果要求数组下标，又不想开新的变量保存答案，那么就用<(因为当l==r时即是答案)
```

