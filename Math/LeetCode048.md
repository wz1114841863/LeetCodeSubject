### 题目：

给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。

来源：力扣（LeetCode）

### 难度：

中等。

### 示例：

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[[7,4,1],[8,5,2],[9,6,3]]
```

### 解题思路：

```
初始想法：
    按规律逐圈处理。
观察规律：
    先上下翻转，再转置。
    如果先转置，后左右翻转就比较麻烦。
```

### 代码实现：

```c++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        //获取维数
        int n = matrix.size();
        //处理特殊情况
        if( n <= 1){
            return ;
        }
        //上下翻转
        for(int i = 0; i < n / 2; i++){
            swap(matrix[i], matrix[ n-1-i]);
        }        
        //转置
        for(int i = 0;  i <= n - 1; i++){
            for( int j = i; j <= n-1; j++){
                swap(matrix[i][j], matrix[j][i]);
            }
        }

        return ;
    }
};
```

### 知识点：

swap真的强。

### 官方代码：

```
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        for (int i = 0; i < n / 2; ++i) {
            for (int j = 0; j < (n + 1) / 2; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n - j - 1][i];
                matrix[n - j - 1][i] = matrix[n - i - 1][n - j - 1];
                matrix[n - i - 1][n - j - 1] = matrix[j][n - i - 1];
                matrix[j][n - i - 1] = temp;
            }
        }
    }
};
```

### 热评：

```
生而为人，我很抱歉。
```

