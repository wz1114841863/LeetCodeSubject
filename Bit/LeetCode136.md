### 题目：

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

### 难度：

简单。

### 示例：

```
输入: [2,2,1]
输出: 1
```

### 解题思路：

```
初始想法：排序后比较。
改进想法：位运算，不会需要额外空间。
```

### 代码实现：

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        //获取长度
        int len = nums.size();
        //遍历，返回最后的结果值
        int res = 0;
        for(int i = 0; i <= len -1 ; i++){
            //cout << res << endl;
            res ^= nums[i];
        }
        return res;
    }
};
```

### 知识点：

看到不需要额外空间就应该考虑位运算。

### 官方代码：

```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ret = 0;
        for (auto e: nums) ret ^= e;
        return ret;
    }
};
```

### 热评：

```
交换律：a ^ b ^ c <=> a ^ c ^ b

任何数于0异或为任何数 0 ^ n => n

相同的数异或为0: n ^ n => 0
```

