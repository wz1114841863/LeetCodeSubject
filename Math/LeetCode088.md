### 题目：

给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。

初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。你可以假设 nums1 的空间大小等于 m + n，这样它就有足够的空间保存来自 nums2 的元素。

### 难度：

简单。

### 示例：

```
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
```

### 解题思路：

```
    思路很简单,轮流比较两个数组的下一个数,哪个数小哪个数就插入.
    但如果原地修改,不能逐次后移,否则时间复杂度会很高.
    可以倒着插入;
        先排大的,从nums1[m-- + n--]开始插入.
```

### 代码实现：

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        //排除特殊情况
        if( n == 0 ){
            return ;
        }else if( m == 0){
            nums1 = nums2;
            return ;
        }
        //倒着遍历数组,找大的插入到末尾.
        int loc = m + n - 1;
        m--; n--;
        while( loc >= 0){
            /*
            cout << "m:  " << m;
            cout << "  n:  " << n;
            cout << "  loc:  " << loc;
            cout << endl;
            */
            if(m < 0){
                nums1[loc] = nums2[n--];
            }else if( n < 0){
                nums1[loc] = nums1[m--];
            }else{
                nums1[loc] = nums1[m] > nums2[n] ? nums1[m--] : nums2[n--];
            }

            loc--;
        }
        
    }
};
```

### 知识点：

三种思路：

​	第一种：直接合并后排序。

​	第二种：新建一个数组，逐渐添加。

​	第三种：从尾部插入。

### 官方代码：

```
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int p1 = m - 1, p2 = n - 1;
        int tail = m + n - 1;
        int cur;
        while (p1 >= 0 || p2 >= 0) {
            if (p1 == -1) {
                cur = nums2[p2--];
            } else if (p2 == -1) {
                cur = nums1[p1--];
            } else if (nums1[p1] > nums2[p2]) {
                cur = nums1[p1--];
            } else {
                cur = nums2[p2--];
            }
            nums1[tail--] = cur;
        }
    }
};
```

### 热评：

```
所有玩家都全力向前冲刺, 却不知道向后才是胜利之门。-《头号玩家》
```

