### 题目：

给你一个未排序的整数数组 `nums` ，请你找出其中没有出现的最小的正整数。

请你实现时间复杂度为 `O(n)` 并且只使用常数级别额外空间的解决方案。

### 难度：

困难。

### 示例：

```
输入：nums = [3,4,-1,1]
输出：2
```

### 解题思路：

```
观察题目，明确指出查找最小的正整数。
第一遍遍历调整位置：
    判断当前数是否为正整数并且小于数组长度。
        如果是，判断是否已经存过这个数
            如果是查找下一个
            如果不是，调整到nums[i].使用swap调用。
        如果不是，下一个数、
第二遍查找，i 是否等于 nums[i]
```

### 代码实现：

```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        //获取数组长度
        int len = nums.size();
        //第一遍排序
        int loc;
        //暂存已经存储过的数
        set<int> temp;
        for(int i = 0; i <= len - 1; ){
            loc = nums[i];
            if(loc > 0  && loc <= len && nums[i] != i + 1){
                //交换位置
                if( !temp.count(loc )){
                    temp.insert(loc);
                    swap( nums[i], nums[loc - 1]);
                }else{
                    i++;
                }
                
            }else{
                i++;
            }
        }
        //测试代码
        /*
        for(int i=0; i<len; i++){
            cout << nums[i] << "   "; 
        }
        */
        //第二趟遍历查找
        for(int i=0; i <= len - 1; i++){
            if( nums[i] != i +  1){
                return i + 1;
            }
        }
        return len + 1;
    }
};
```

### 知识点：

本题比较关键的点在于对时间复杂度为O(n)和空间复杂度为O(1)的限制。

官方解答中用了哈希表来实现。

### 官方代码：

```
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        for (int& num: nums) {
            if (num <= 0) {
                num = n + 1;
            }
        }
        for (int i = 0; i < n; ++i) {
            int num = abs(nums[i]);
            if (num <= n) {
                nums[num - 1] = -abs(nums[num - 1]);
            }
        }
        for (int i = 0; i < n; ++i) {
            if (nums[i] > 0) {
                return i + 1;
            }
        }
        return n + 1;
    }
};
```

### 热评：

```
管你啥时间空间复杂度呢，打卡就完事了
```

```
方法一这个哈希真的是妙蛙种子什么什么米奇妙妙屋妙到家了。。。
```

