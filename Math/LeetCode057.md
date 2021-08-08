### 题目：

给你一个 **无重叠的** *，*按照区间起始端点排序的区间列表。

在列表中插入一个新的区间，你需要确保列表中的区间仍然有序且不重叠（如果有必要的话，可以合并区间）。

### 难度：

中等。

### 示例：

```
输入：intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
输出：[[1,2],[3,10],[12,16]]
解释：这是因为新的区间 [4,8] 与 [3,5],[6,7],[8,10] 重叠。
```

### 解题思路：

```
有序。先查找newInterval[0]所在的区间。再查找newInterval[1]所在的区间。
应该属于暴力破解法。
```

### 代码实现：

```c++
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        //获取向量长度
        int len = intervals.size();
        //返回结果
        vector<vector<int>> res;
        //排除特殊情况
        if( len == 0){
            res.push_back(newInterval);
            return res;
        }
        //遍历查找
        //查找左边点
        vector<int> temp(2,0);
        int i = 0;
        for( i=0; i<len; i++){
            if(newInterval[0] <= intervals[i][0]){
                temp[0] = newInterval[0];
                break;
            }else if( newInterval[0] <= intervals[i][1]){
                temp[0] = intervals[i][0];
                break;
            }else{
                temp = intervals[i];
                res.push_back(temp);
            }
        }
        //查找右边点
        if( i == len){
            res.push_back(newInterval);
            return res;
        }
        for(i; i<len; i++){
            if(newInterval[1] < intervals[i][0]){
                temp[1] = newInterval[1];
                res.push_back(temp);
                temp[1] = -1;
                break;
            }else if( newInterval[1] <= intervals[i][1] ){
                temp[1] = intervals[i][1];
                res.push_back(temp);
                i++;
                temp[1] = -1;               
                break;
            }
        }
        //加入剩余节点
        if( i == len && temp[1] != -1){
            temp[1] = newInterval[1];
            res.push_back(temp);
            return res;
        }
        for(i; i<len; i++){
            temp = intervals[i];
            res.push_back(temp);
        }
        return res;
    }
};
```

### 知识点：

时间复杂度都是O(n)，区别还是在于代码和算法的简洁程度上。

### 官方代码：

```
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        int left = newInterval[0];
        int right = newInterval[1];
        bool placed = false;
        vector<vector<int>> ans;
        for (const auto& interval: intervals) {
            if (interval[0] > right) {
                // 在插入区间的右侧且无交集
                if (!placed) {
                    ans.push_back({left, right});
                    placed = true;                    
                }
                ans.push_back(interval);
            }
            else if (interval[1] < left) {
                // 在插入区间的左侧且无交集
                ans.push_back(interval);
            }
            else {
                // 与插入区间有交集，计算它们的并集
                left = min(left, interval[0]);
                right = max(right, interval[1]);
            }
        }
        if (!placed) {
            ans.push_back({left, right});
        }
        return ans;
    }
};
```

### 热评：

```
早安，划水人，划水使我们在这里相遇。
```

