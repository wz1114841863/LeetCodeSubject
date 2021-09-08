## 两数之和

```
//最优解法：使用哈希表作为查找表。遍历的同时记录节点信息。
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        //定义哈希表
        unordered_map<int, int> m1;
        //使用for(item : vector)的话无法返回数组下标。
        for(int i = 0; i<nums.size(); i++){
            auto iter = m1.find( target - nums[i]);
            //查找是否成功
            if( iter != m1.end() ){
                //返回的应该是initiaizer_list
                return {iter->second, i};
            }
            m1[nums[i]] = i;
        }
        return {};
    }
};
```

