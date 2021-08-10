### 题目：

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

### 难度：

中等。

### 示例：

```
输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
输出：2
解释：
3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右

```

### 解题思路：

```
结合上一题，考虑障碍物的情况即可。
```

### 代码实现：

```c++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        //获取m,n
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        //排除特殊情况
        if( obstacleGrid[0][0] == 1 || obstacleGrid.empty() || obstacleGrid[m-1][n-1] == 1){
            return 0;
        }
        //根据上边和右边的格子，推出到达当前格子的可能方法。
        int count[m][n];
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                if( i == 0 && j == 0){
                    count[i][j] = 1;
                }else if( i == 0){
                    //最上边
                    count[i][j] = obstacleGrid[i][j-1] == 1 ? 0 : count[i][j-1];
                }else if( j == 0){
                    //最右边                    
                    count[i][j] = obstacleGrid[i - 1][j] == 1 ? 0 : count[i - 1][j];

                }else{
                    //其他情况
                    if( obstacleGrid[i-1][j] == 1 && obstacleGrid[i][j-1] == 1){
                        count[i][j] = 0;
                    }else if( obstacleGrid[i-1][j] == 1 ){
                        count[i][j] = count[i][j-1];
                    }else if( obstacleGrid[i][j-1] == 1 ){
                        count[i][j] = count[i-1][j];
                    }else{
                        count[i][j] = count[i-1][j] + count[i][j-1];                        
                    }
                }
            }
        }
        return count[m-1][n-1];        
    }
};
```

### 知识点：

这道题就只能用动态规划来做了。

### 官方代码：

```
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int n = obstacleGrid.size(), m = obstacleGrid.at(0).size();
        vector <int> f(m);

        f[0] = (obstacleGrid[0][0] == 0);
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (obstacleGrid[i][j] == 1) {
                    f[j] = 0;
                    continue;
                }
                if (j - 1 >= 0 && obstacleGrid[i][j - 1] == 0) {
                    f[j] += f[j - 1];
                }
            }
        }

        return f.back();
    }
};
```

### 热评：

```
解法倒是很简单，但是会有傻x把障碍放在入口？？？？？？？？？？？？？
```

