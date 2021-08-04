### 题目：

给你一个非负整数数组 nums ，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

假设你总是可以到达数组的最后一个位置。

### 难度：

中等。

### 示例：

```
输入: nums = [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```

### 解题思路：

```
关键在于如何选择下一步所跳的位置
    1.步数必须小于等于 nums[i]。
    2.同时还要考虑让下一步尽可能的多跳。
    for(j=i+1; j <= i + nums[i]; j++ ){
        判断 j + nums[j] 的 最大值。
    }
```

### 代码实现：

```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        //获取数组长度
        int len = nums.size();
        //排除特殊情况
        if(len == 1 ){
            return 0;
        }
        //贪心遍历
        int number = 0;
        //可跳跃的最大值
        int max, temp;
        int loc = 0;
        //记录位置
        for(int i=0; i <= len -1; ){
            //判断是否结束
            if(nums[i] + i >= len - 1){
                number++;
                break;
            }
            loc = i;
            max = nums[i] + i;
            //从当前位置一直到 i+nums[i]
            for(int j = i+1 ; j <= i + nums[i]; j++){
                temp = nums[j] + j;
                if( temp >= max){
                    max = temp;
                    loc = j;
                }
            }
            number++;
            i=loc;
        }
        return number;
    }
};
```

### 知识点：

贪心算法，尽可能的多的向前走。

### 官方代码：

```
class Solution {
    public int jump(int[] nums) {
        int length = nums.length;
        int end = 0;
        int maxPosition = 0; 
        int steps = 0;
        for (int i = 0; i < length - 1; i++) {
            maxPosition = Math.max(maxPosition, i + nums[i]); 
            if (i == end) {
                end = maxPosition;
                steps++;
            }
        }
        return steps;
    }
}
```

### 热评：

```
动态规划添加了这一句，留下了没有技术含量的泪水

if(nums[0]==25000)return 2;
```

```
"每天起床第一句，先去看看力扣题。自从有了打卡题，每天变得更努力。写完看看扣友圈，题解视频有魔力。打卡，我要打卡，我要编程万人敌。《燃烧我的力扣题》"
```

