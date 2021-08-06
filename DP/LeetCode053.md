### 题目：

给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

### 难度：

简单。

### 示例：

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

### 解题思路：

```
temp < 0:
对下一个数的：
    >= 0:
        直接赋值temp：  
    <0:
       大于 temp?   temp = nums[i]。      
       小于temp？ 下一个数           
temp > 0
对下一个数处理：
    >= 0:
        直接相加。 temp 与 max 做对比
    <0:
        绝对值大于 temp?  temp 清零。找下一个正数。
        绝对值小于temp？ temp + nums[i]。
```

### 代码实现：

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        //获取数组长度
        int len = nums.size();
        //判断特殊情况
        if( len == 1){
            return nums[0];
        }
        //遍历数组
        int maxSum = nums[0]; 
        int temp = nums[0];
        for(int i = 1; i < len; i++){
            //分类讨论
            if(temp < 0){
                if( nums[i] >= temp){
                    temp = nums[i];
                    maxSum = max(maxSum, temp);
                }
            }else{
                if( nums[i] >= 0){
                    temp = temp + nums[i];
                    maxSum = max(maxSum, temp);
                }else if( nums[i] < 0 && temp > (- nums[i])){
                     temp = temp + nums[i];
                }else if( nums[i] < 0 && temp <= (- nums[i])){
                    temp = 0;
                } 
            }
        }
        return  maxSum;
    }
};
```

### 知识点：

我使用的应该属于暴力破解。还有更巧妙的动态规划和分治法。只是我不会。菜狗.jpg。

### 官方代码：

```
class Solution {
public:
    struct Status {
        int lSum, rSum, mSum, iSum;
    };

    Status pushUp(Status l, Status r) {
        int iSum = l.iSum + r.iSum;
        int lSum = max(l.lSum, l.iSum + r.lSum);
        int rSum = max(r.rSum, r.iSum + l.rSum);
        int mSum = max(max(l.mSum, r.mSum), l.rSum + r.lSum);
        return (Status) {lSum, rSum, mSum, iSum};
    };

    Status get(vector<int> &a, int l, int r) {
        if (l == r) {
            return (Status) {a[l], a[l], a[l], a[l]};
        }
        int m = (l + r) >> 1;
        Status lSub = get(a, l, m);
        Status rSub = get(a, m + 1, r);
        return pushUp(lSub, rSub);
    }

    int maxSubArray(vector<int>& nums) {
        return get(nums, 0, nums.size() - 1).mSum;
    }
};
。
```

### 热评：

```
这就是简单题吗？ai了ai了
```

```
看到这题难度为简单，我陷入了沉思! 思考了30分钟我一行代码没写出来，我又陷入了沉思。
```

