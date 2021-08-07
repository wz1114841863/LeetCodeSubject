### 题目：

给定一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标。

### 难度：

中等。

### 示例：

```
输入：nums = [2,3,1,1,4]
输出：true
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
```

### 解题思路：

```
    末尾最后一个元素是什么不重要。
    最特殊的只有为0的地方。
    每一步先按当前能走的最大步数走
        如果越界，证明可以走到终点，只不过当前步伐太大而已
        如果遇到某一格为0，证明此格不能走，因为赐个怎么走都不能走到最后，最大步数减一。
        如果能走的步数已经递减为0，将此数置为0，返回上一个数，从新开始递减步数。

以上步骤并没有做到贪心。只能算是暴力破解法，所以会 超出时间限制
优化：
    应该去找nums[i]所能走的下一步范围中所能走的最大步伐的位置。
    关键在于如何选择下一步所跳的位置
    1.步数必须小于等于 nums[i]。
    2.同时还要考虑让下一步尽可能的多跳。
    for(j=i+1; j <= i + nums[i]; j++ ){
        判断 j + nums[j] 的 最大值。
    }
```

### 代码实现：

```c++
/*
暴力破解法：
class Solution {
public:
    bool canJump(vector<int>& nums) {
        //获取长度
        int len = nums.size();
        //处理特殊情况
        if( len == 1){
            return true;
        }
        //用来存储所有走过的数。
        stack<int> location;
        location.push(0);
        int loc;
        int temp;
        while( !location.empty() ){
            //获取栈顶元素。
            loc = location.top();
            temp = loc + nums[loc];
            //判断对应位置的数字是否为零
            if(nums[loc] == 0){
                location.pop();
                continue;
            }
            //判断是否可以到达末尾
            if( loc + nums[loc] >= len - 1){
                //可以
                return true;
            }
            //nums[i] != 0 同时也不能到达末尾，判断跳转的数是否存在
            if( loc + nums[loc] != 0 ){
                location.push(loc + nums[loc]);
                cout << loc + nums[loc] << endl;
            }
            nums[loc]--;
        }
        return false;
    }
};
*/

class Solution {
public:
    bool canJump(vector<int>& nums) {
        //获取长度
        int len = nums.size();
        //排除特殊情况 
        if( len == 1){
            //长度为1时，已经到达最后一个坐标。
            return true;
        }
        //贪心遍历
        //可跳跃的最大值
        int max, temp;
        int loc = 0;
        //记录位置
        for(int i=0; i <  len -1; ){
            //判断是否结束
            if(nums[i] + i >= len - 1){
                return true;
            }else if( nums[i] == 0){
                return false;
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
            i=loc;
        }
        return false;
    }
};
```

### 知识点：

最开始还以为自己写的就是贪心算法。流下了不学无术的眼泪。

### 官方代码：

```
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int n = nums.size();
        int rightmost = 0;
        for (int i = 0; i < n; ++i) {
            if (i <= rightmost) {
                rightmost = max(rightmost, i + nums[i]);
                if (rightmost >= n - 1) {
                    return true;
                }
            }
        }
        return false;
    }
};。
```

### 热评：

```
为什么我这么蠢啊。。。。。。。
```

