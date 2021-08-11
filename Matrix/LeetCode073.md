### 题目：

给定一个 m x n 的矩阵，如果一个元素为 0 ，则将其所在行和列的所有元素都设为 0 。请使用 原地 算法。

进阶：

一个直观的解决方案是使用  O(mn) 的额外空间，但这并不是一个好的解决方案。
一个简单的改进方案是使用 O(m + n) 的额外空间，但这仍然不是最好的解决方案。
你能想出一个仅使用常量空间的解决方案吗？

### 难度：

中等。

### 示例：

```
输入：matrix = [[1,1,1],[1,0,1],[1,1,1]]
输出：[[1,0,1],[0,0,0],[1,0,1]]
```

### 解题思路：

```
如果matrix[m][n] == 0, 设置 matrix[m][0] = matrix[0][n] = 0;
第一行和第一列要特殊对待。
```

### 代码实现：

```c++
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        //获取矩阵m，n
        int m = matrix.size();
        int n = matrix[0].size();
        //遍历查找0
        //第一行和第一列比较特殊
        int row = 0, col = 0;
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                if(matrix[i][j] == 0){
                    if(i == 0 && j == 0){
                        row = 1;
                        col = 1;
                    }else if( i == 0 ){
                        row = 1;
                    }else if( j == 0){
                        col = 1;
                    }else{
                        matrix[i][0] = 0;
                        matrix[0][j] = 0;
                    }
                }
            }
        }
        //矩阵置零
        for(int i=1; i<m; i++){
            if(matrix[i][0] == 0){
                for(int j=0; j<n; j++){
                    matrix[i][j] = 0;
                }
            }
        }
        for(int j=1; j<n; j++){
            if(matrix[0][j] == 0){
                for(int i=0; i<m; i++){
                    matrix[i][j] = 0;
                }
            }
        }
        if(row == 1){
            for(int j=0; j<n; j++){
                matrix[0][j] = 0;
            }
        }
        if(col == 1){
            for(int i=0; i<m; i++){
                matrix[i][0] = 0;
            }
        }
        return ;
    }
};
```

### 知识点：

与其说是利用常量空间，不如说是利用现有矩阵。

### 官方代码：

```
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        int flag_col0 = false;
        for (int i = 0; i < m; i++) {
            if (!matrix[i][0]) {
                flag_col0 = true;
            }
            for (int j = 1; j < n; j++) {
                if (!matrix[i][j]) {
                    matrix[i][0] = matrix[0][j] = 0;
                }
            }
        }
        for (int i = m - 1; i >= 0; i--) {
            for (int j = 1; j < n; j++) {
                if (!matrix[i][0] || !matrix[0][j]) {
                    matrix[i][j] = 0;
                }
            }
            if (flag_col0) {
                matrix[i][0] = 0;
            }
        }
    }
};
```

### 热评：

```
又到了我最爱的CV环节。
```

