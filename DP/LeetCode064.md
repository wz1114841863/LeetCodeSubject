### 题目：

给定一个包含非负整数的 `*m* x *n*` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

### 难度：

中等。

### 示例：

```
输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。
```

### 解题思路：

```
仿照前两题，仅保留能到当前格的最小数字总和。
```

### 代码实现：

```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        //获取m，n
        int m = grid.size();
        int n = grid[0].size();
        //根据上边和右边的格子
        //记录当前格的最小数字总和
        int count[m][n];
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                if( i == 0 && j == 0){
                    count[i][j] = grid[i][j];
                }else if( i == 0){
                    //最上边
                    count[i][j] = count[i][j-1] + grid[i][j];
                }else if( j == 0){
                    //最右边                    
                    count[i][j] = count[i-1][j] + grid[i][j];

                }else{
                    //其他情况
                    count[i][j] = min(count[i-1][j], count[i][j-1]) + grid[i][j];                        
                }
            }
        }  
        return count[m-1][n-1]   ;   
    }
};
```

### 知识点：

经典动态规划。

### 官方代码：

```
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        if (grid.size() == 0 || grid[0].size() == 0) {
            return 0;
        }
        int rows = grid.size(), columns = grid[0].size();
        auto dp = vector < vector <int> > (rows, vector <int> (columns));
        dp[0][0] = grid[0][0];
        for (int i = 1; i < rows; i++) {
            dp[i][0] = dp[i - 1][0] + grid[i][0];
        }
        for (int j = 1; j < columns; j++) {
            dp[0][j] = dp[0][j - 1] + grid[0][j];
        }
        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < columns; j++) {
                dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
            }
        }
        return dp[rows - 1][columns - 1];
    }
};
```

### 热评：

```
有点不习惯，给我整个难点的。
```

