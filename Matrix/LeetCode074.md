### 题目：

编写一个高效的算法来判断 m x n 矩阵中，是否存在一个目标值。该矩阵具有如下特性：

每行中的整数从左到右按升序排列。
每行的第一个整数大于前一行的最后一个整数。

### 难度：

中等。

### 示例：

```
输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
输出：true
```

### 解题思路：

```
先查找在哪一行，再利用二分查找查找在哪一列。
```

### 代码实现：

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        //获取matrix的维度
        int m = matrix.size();
        int n = matrix[0].size();
        //排除特殊情况
        if(target < matrix[0][0] || target > matrix[m-1][n-1]){
            return false;
        }
        //确定行
        int i;
        for(i=m-1; i>=0; i--){
            if(matrix[i][0] <= target){
                //获取i
                break;
            }
        }
        //获取列
        int left = 0, right = n-1, middle;
        int temp;
        while(left<=right){
            middle = (left + right) / 2;
            temp = matrix[i][middle];
            if(temp == target){
                return true;
            }else if(temp < target){
                left = middle + 1;
            }else if(temp > target){
                right = middle - 1;
            }
        }
        return false;
    }
};
```

### 知识点：

第一次查找也可以使用二分查找。

### 官方代码：

```
class Solution {
public:
    bool searchMatrix(vector<vector<int>> matrix, int target) {
        auto row = upper_bound(matrix.begin(), matrix.end(), target, [](const int b, const vector<int> &a) {
            return b < a[0];
        });
        if (row == matrix.begin()) {
            return false;
        }
        --row;
        return binary_search(row->begin(), row->end(), target);
    }
};
```

### 热评：

```
我直接return rand()%2; 总会有通过那一天。
```

