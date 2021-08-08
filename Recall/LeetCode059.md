### 题目：

给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix` 。

### 难度：

中等。

### 示例：

```
给你一个正整数 n ，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix 。
```

### 解题思路：

```
递归插入。
```

### 代码实现：

```c++
class Solution {
private:
    void matrixPrint(vector<vector<int>>& matrix, int n, int nloc, int &count){
        if( n == 0 ){
            return ;
        }
        //n == 1
        if( n == 1 ){
            matrix[nloc][nloc] = count;
            count++;
            return ;
        }
        //遍历最外面的一层
        for(int i=0; i<n; i++){
            matrix[nloc][nloc + i] = count;
            count++;
        }
        for(int i=1; i<n-1; i++){
            matrix[nloc + i][nloc + n - 1] = count;
            count++;
        }
        for(int i=n-1; i>=0; i--){
            matrix[nloc + n - 1][nloc + i] = count;
            count++;
        }
        for(int i=n-2; i>=1; i--){
            matrix[nloc + i][nloc] = count;
            count++;
        }
        matrixPrint(matrix, n - 2, nloc + 1, count);

    }
public:
    vector<vector<int>> generateMatrix(int n){
        //初始化返回结果。
        vector<vector<int>> matrix(n, vector<int>(n,0));
        //初始位置
        int nloc = 0;
        int count = 1;
        matrixPrint(matrix, n, nloc, count);
        return matrix;
    }
};
```

### 知识点：

仿照题目 **螺旋矩阵** 改动。

### 官方代码：

```
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        int num = 1;
        vector<vector<int>> matrix(n, vector<int>(n));
        int left = 0, right = n - 1, top = 0, bottom = n - 1;
        while (left <= right && top <= bottom) {
            for (int column = left; column <= right; column++) {
                matrix[top][column] = num;
                num++;
            }
            for (int row = top + 1; row <= bottom; row++) {
                matrix[row][right] = num;
                num++;
            }
            if (left < right && top < bottom) {
                for (int column = right - 1; column > left; column--) {
                    matrix[bottom][column] = num;
                    num++;
                }
                for (int row = bottom; row > top; row--) {
                    matrix[row][left] = num;
                    num++;
                }
            }
            left++;
            right--;
            top++;
            bottom--;
        }
        return matrix;
    }
};
```

### 热评：

```
啪的一下把昨天的代码拿过来就过了啊，很快啊。
```

