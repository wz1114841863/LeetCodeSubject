### 题目：

给定一个已按照 非递减顺序排列  的整数数组 numbers ，请你从数组中找出两个数满足相加之和等于目标数 target 。

函数应该以长度为 2 的整数数组的形式返回这两个数的下标值。numbers 的下标 从 1 开始计数 ，所以答案数组应当满足 1 <= answer[0] < answer[1] <= numbers.length 。

你可以假设每个输入 只对应唯一的答案 ，而且你 不可以 重复使用相同的元素。

### 难度：

简单。

### 示例：

```
输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。
```

### 解题思路：

```
    丛数组两边开始查找。
    如果和相加大于target，右指针左移。
    如果和相加小于target，左指针右移
    直到两指针相遇。
```

### 代码实现：

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        //返回结果
        vector<int> res;
        int left = 0, right = numbers.size() - 1;
        int sum = 0;
        while( left != right){
            sum = numbers[left] + numbers[right];
            if(sum == target){
                res.push_back(left+1);
                res.push_back(right+1);

                return res;
            }else if( sum > target){
                right--;
            }else if(sum < target){
                left++;
            }
        }

        return res;
    }
};
```

### 知识点：

考虑向量的非递减特性，很容易考虑出从两边向中间集合。

### 官方代码：

```
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int low = 0, high = numbers.length - 1;
        while (low < high) {
            int sum = numbers[low] + numbers[high];
            if (sum == target) {
                return new int[]{low + 1, high + 1};
            } else if (sum < target) {
                ++low;
            } else {
                --high;
            }
        }
        return new int[]{-1, -1};
    }
}
```

### 热评：

```
首先我们考虑动态规划…不用啊，我谢谢你。
```

