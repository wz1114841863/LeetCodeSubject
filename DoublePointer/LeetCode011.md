### 题目：

给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0) 。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

### 难度：

中等。

### 示例：

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

### 解题思路：

```
初始思路：
首尾两个指针，计算面积向中间并拢。
并拢条件：
1.哪个指针所指向的数小就移动。
2.移动后判断是否有原来所指向的数大、
    如果有计算面积比较，接着返回第一步。
    如果没有当前指针就继续移动。
3.若两指针相遇，遍历结束。
```

### 代码实现：

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        //数组长度大于2.因此不用特意判断len = 0 或 1 的情形。
        int len = height.size();
        //最大面积
        int maxArea = 0;
        int temp = 0;
        //头尾指针。
        int header = 0, toiler = len-1;
        while( header != toiler){
            if( height[header] <= height[toiler]){
                temp = height[header] * (toiler - header);
                header++;
                maxArea = max(maxArea, temp);
            }
            else{
                temp = height[toiler] * (toiler - header);
                toiler--;
                maxArea = max(maxArea, temp);
            }
        }
        return maxArea;
    }
};
```

### 知识点：

这道题的思路比解题更重要，利用双指针来解题。

或许存在一些博弈的想法。其中短板是影响面积的因素。所以更应该去移动短板而不是长板。

### 官方代码：

```
class Solution {
public:
    int maxArea(vector<int>& height) {
        int l = 0, r = height.size() - 1;
        int ans = 0;
        while (l < r) {
            int area = min(height[l], height[r]) * (r - l);
            ans = max(ans, area);
            if (height[l] <= height[r]) {
                ++l;
            }
            else {
                --r;
            }
        }
        return ans;
    }
};

```

### 热评：

```
你用你的双指针，我有我的两个for
```

```
看了题解，双指针，从两头开始内卷，先卷了挫的那头
```

