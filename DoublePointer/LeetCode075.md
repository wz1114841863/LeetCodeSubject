### 题目：

给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

### 难度：

中等。

### 示例：

```
输入：nums = [2,0,2,1,1,0]
输出：[0,0,1,1,2,2]
```

### 解题思路：

```
三个指针：
    第一个指针指向0的尾部位置加一。
    第二个指针遍历向量。
    第三个指向2的头部位置减一。
```

### 代码实现：

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        //获取长度
        int len = nums.size();
        //排除特殊情况
        if(len == 1){   
            return ;
        }
        //遍历
        int first = 0;
        int second = 0;
        int third = len - 1;
        while(second <= third){
            if(nums[second] == 0){
                swap(nums[first], nums[second]);
                first ++;
                second++;
            }else if(nums[second] == 2){
                swap(nums[third], nums[second]);
                third--;
            }else{
                second++;
            }
        }
        return ;
    }
};
```

### 知识点：

直接理解为排序即可。

### 官方代码：

```
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int n = nums.size();
        int p0 = 0, p2 = n - 1;
        for (int i = 0; i <= p2; ++i) {
            while (i <= p2 && nums[i] == 2) {
                swap(nums[i], nums[p2]);
                --p2;
            }
            if (nums[i] == 0) {
                swap(nums[i], nums[p0]);
                ++p0;
            }
        }
    }
};
```

### 热评：

```
我上来就是一个sort(nums.begin(),nums.end());
```

