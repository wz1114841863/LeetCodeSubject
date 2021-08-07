### 题目：

以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。请你合并所有重叠的区间，并返回一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间。

### 难度：

中等。

### 示例：

```
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```

### 解题思路：

```
排序后再合并。
```

### 代码实现：

```c++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        //获取向量长度
        int len = intervals.size();
        //排除特殊情况
        if( len == 0 || len == 1){
            return intervals;
        }
        //排序
        sort(intervals.begin(), intervals.end());
        //排序后遍历
        vector<vector<int>> res;
        vector<int> temp, loc;
        int count = 0;
        loc = intervals[0];
        res.push_back(loc);
        for(int i=1; i<len; i++){
            loc = res[count];
            temp = intervals[i];
            //判断前一区间的末尾小于后一区间的开头
            if( loc[1] < temp[0]){
                //小于，则新建区间
                res.push_back(temp);
                count++;
            }else{
                //不小于，则合并区间
                loc[1] = max(loc[1], temp[1]);
                //修改区间
                res[count] = loc;
            }
        }
        return res;
    }
};
```

### 知识点：

sort可以排序vector<vector<int>>.

### 官方代码：

```
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        if (intervals.size() == 0) {
            return {};
        }
        sort(intervals.begin(), intervals.end());
        vector<vector<int>> merged;
        for (int i = 0; i < intervals.size(); ++i) {
            int L = intervals[i][0], R = intervals[i][1];
            if (!merged.size() || merged.back()[1] < L) {
                merged.push_back({L, R});
            }
            else {
                merged.back()[1] = max(merged.back()[1], R);
            }
        }
        return merged;
    }
};
```

### 热评：

```
字节面试官: 先出道基础的考考你.
```

