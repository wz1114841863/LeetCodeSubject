### 题目：

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

### 难度：

中等。

### 示例：

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
输入：nums = [0]
输出：[]
```

### 解题思路：

```
初步想法：
数组中可能有正数、负数和零。也可能有重复数字。
将当前数组建立为multimap，然后利用双指针，从头到尾遍历所有可能的组合。
计算两个数的和，进一步在multimap中查找是哦福存在第三个数，使得三个数和为零。
查询到的数查看是都是当前的两个数，如果是放弃这个数，如果不是，存储这三个数。
遍历完成后，对存储的数组进行删重。删掉所有重复的三元组。

改进一下：
存储的数组是 vector<vector<int>>，不知道怎么能删重，而且花费的时间代价太大，
很容易超时，为了避免这种情况，可以考虑先给数组排序，再遍历数组查找结果。
1.如果nums的长度小于3，直接返回空。
2.对nums进行排序，由小到大。
3.排序后判断第一个numsp[0]是否小于零以及nums[size-1]是否大于0，如果不能同时满足，直接返回空。
4.遍历查找可能存在的结果：
    1)判断当前数字与上一个数字是否一样，如果是跳过该数字。反之执行下一步。
        因为可鞥出现全零数组这样的情况，以及[-1,-1,2]这样子的解的情况，所以应该向前比较，而不是向后比较
        否则会出现漏解。
    2)left = i+1，Right = size-1
    判断 nums[i] + nums[left] + nums[right] == 0?
        如果是，右移left，左移right，同时保证与原先值不同，这样才能保证不会出现重复解。
        如果不是，判断当前和 sum 与 0 的大小关系。
            大于0，说明Right太大，左移
            小于0，说明left太小，右移
        边界条件： left ！= right，遍历结束

5.返回result。
注意：防止越界！！！
```

### 代码实现：

```c++
/*
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        //返回的结果是vector<vector<int>> 类型的
        vector<vector<int>> res;
        //先判断nums中元素的个数，如果小于3，直接返回空。
        if( nums.size() < 3){
            return res;
        }
        //存储当前向量到multimap中去。
        //key存储数组值， value存储序号
        //key用来表示是当前数字可与另两个数字相加为0， Value用来保证查找到的值不是其余两个数本身。
        multimap<int, int> multimapNums;
        for(int i=0; i<nums.size(); i++){
            multimapNums.insert(nums[i], i);
        }
        //排序

        //利用双指针实现所有组合的遍历
        int temp = 0;
        vector<int> target;
        typedef multimap< int,int >::iterator iterator;
        iterator iter;
        pair< iterator, iterator > pos;
        for(int  i=0; i<nums.size() - 1; i++){
            for(int j=i+1; j<nums.size(); j++){
                temp = 0 - (nums[i] + nums[j]);
                //查找，如果存在，count返回值不为零。如果不存在，返回0。
                switch(multimapNums.count(temp)){
                    //不存在，跳过，执行下一次查找。
                    case 0:
                        break;
                    //只存在一处，使用find即可。
                    case 1:{
                        iter = multimapNums.find( temp );
                        //排除重复使用同一个位置的数字。
                        if( (iter.second != i) && (iter.second != j)){
                            //target向量清空，用来存放查询出来的结果。
                            target.clear();
                            //插入元素
                            terger.insert(nums[i]);
                            target.insert(nums[j]);
                            target.insert(iter.first);
                            //插入结果
                            res.insert(target);
                        }
                        break;
                    }
                    //存在多处，使用equal_range:
                    default:{
                        pos = multimapNums.equal_range( temp );
                        for ( ; pos.first != pos.second; pos.first++ ){
                            // 对每个元素进行操作
                            iter = pos.first;
                            if( (iter.second != i) && (iter.second != j)){
                                //target向量清空，用来存放查询出来的结果。
                                target.clear();
                                //插入元素
                                terger.insert(nums[i]);
                                target.insert(nums[j]);
                                target.insert(iter.first);
                                //插入结果
                                res.insert(target);
                            }
                        }
                    }
                }


            }
        }
        //对result进行删重。
		//未实现
    }
};
*/
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        //返回结果为vector<vector<int>>类型的值。
        vector<vector<int>> result;
        //第一步判断向量长度是否大于3
        if( nums.size() < 3 ){
            return result;
        }
        //对向量进行排序。利用了C++中的sort函数。
        sort(nums.begin(), nums.end());
        //判断是否可能有解
        //最小的数大于0，或最大的数小于零都说明没解。直接返回空。
        int len = nums.size();
        int left, right;
        int sum;
        vector<int> target;
        //特殊情况。
        if((nums[0] > 0) || (nums[len -1]) < 0){
            return result;
        }
        else if( len == 3){
            sum = nums[0] + nums[1] + nums[2];
            if(sum == 0){
                result.push_back(nums);    
            }
            return result;
        }
        //可能有解即遍历
        //从第一个数查到倒数第三个数。
        for(int i=0; i < len - 2; ){

            //定位left, right.
            left = i + 1;
            right = len - 1;
            //边界条件
            while( left < right ){
                sum = nums[i] + nums[left] + nums[right];
                //等于0.记录这一组数据。left，right移位
                if( sum == 0 ){
                    target.clear();
                    target.push_back(nums[i]);
                    target.push_back(nums[left]);
                    target.push_back(nums[right]);
                    result.push_back(target);
                    left++;
                    while((left <= right) && (nums[left] == nums[left - 1]) ){
                        left++;
                    }
                    right--;
                    while( (right >= left) && (nums[right] == nums[right + 1]) ){
                        right--;
                    }

                }
                //小于0，left太小了，右移至下一个不相同的数。  
                else if( sum < 0 ){
                    left++;
                    while( (left <= right) && (nums[left] == nums[left - 1]) ){
                        left++;
                    }
                }
                else if( sum > 0 ){
                    right--;
                    while( (right >= left) && (nums[right] == nums[right + 1])){
                        right--;
                    }
                }
            }
            i++;
            //先判断当前数字与前面的数字是否一样
            //一样则跳过。否则会造成重复解。
            while((i < len - 2) && (nums[i] == nums[i-1])){
                i++;
            }
        }
        return result;
    }
};
```

### 知识点：

可以看到第二种想法巧妙的避开了数组去重的问题。而且也不用笨拙的遍历所有的数。效率和复杂度都有一定的提升。用双指针的思想能极大的简化一些步骤。

### 官方代码：

```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        vector<vector<int>> ans;
        // 枚举 a
        for (int first = 0; first < n; ++first) {
            // 需要和上一次枚举的数不相同
            if (first > 0 && nums[first] == nums[first - 1]) {
                continue;
            }
            // c 对应的指针初始指向数组的最右端
            int third = n - 1;
            int target = -nums[first];
            // 枚举 b
            for (int second = first + 1; second < n; ++second) {
                // 需要和上一次枚举的数不相同
                if (second > first + 1 && nums[second] == nums[second - 1]) {
                    continue;
                }
                // 需要保证 b 的指针在 c 的指针的左侧
                while (second < third && nums[second] + nums[third] > target) {
                    --third;
                }
                // 如果指针重合，随着 b 后续的增加
                // 就不会有满足 a+b+c=0 并且 b<c 的 c 了，可以退出循环
                if (second == third) {
                    break;
                }
                if (nums[second] + nums[third] == target) {
                    ans.push_back({nums[first], nums[second], nums[third]});
                }
            }
        }
        return ans;
    }
};
```

### 热评：

```
一顿操作猛如虎，点击提交超时了。

二话不说翻题解，评论区里全人才。

反反复复终得道，再次尝试却报错。

行行检查字字改，击败用户百分五。
```

```
一题二写，三数之和，题解四瞅五瞄六瞧，水平还七上八下九流，十分辣鸡。
十推九敲，八种思路，用光七情六欲五感，在这里四覆三翻二挠，一拳爆屏。
一天两道，三月打卡，刷的四分五裂六离，现在是七零八落九散，十分难受。
leetcode里，题不会，解不懂，小小码农，可笑可笑！
```

