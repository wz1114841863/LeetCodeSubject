### 题目：

给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

### 难度：

中等。

### 示例：

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

### 解题思路：

```
找规律：
    逐层遍历 + 递归。
```

### 代码实现：

```c++
class Solution {
private:
    void spiralOrderPrint(vector<vector<int>>& matrix, int m, int n, int mloc, int nloc, vector<int> &res){
        if( n == 0 || m == 0){
            return ;
        }
        //n == 1 || m==1
        if( n == 1 ){
            for(int i=0; i<m; i++){
                res.push_back( matrix[ mloc + i][nloc] ) ;
            }
            return ;
        }else if( m == 1 ){
            for( int i=0; i<n; i++){
                res.push_back( matrix[mloc][nloc + i] );
            }
            return ;
        }
        //遍历最外面的一层
        for(int i=0; i<n; i++){
            res.push_back(matrix[mloc][nloc + i]);
        }
        for(int i=1; i<m-1; i++){
             res.push_back(matrix[mloc + i][nloc + n - 1]);
        }
        for(int i=n-1; i>=0; i--){
             res.push_back(matrix[mloc + m - 1][nloc + i]);
        }
        for(int i=m-2; i>=1; i--){
             res.push_back(matrix[mloc + i][nloc]);
        }
        spiralOrderPrint(matrix, m -2 , n - 2, mloc + 1, nloc + 1, res);

    }
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        //获取m，n （>=1）
        int m = matrix.size();
        int n = matrix[0].size();
        //返回结果。
        vector<int> res;
        //初始位置
        int mloc = 0, nloc = 0;
        spiralOrderPrint(matrix, m, n, mloc, nloc, res);
        return res;
    }
};
```

### 知识点：

目前看到三种解法：

​	1、逐层遍历，可用递归也可以不用。

​	2、记录方向。

​	3、”剃头“，然后旋转矩阵。

### 官方代码：

```
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> order = new ArrayList<Integer>();
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return order;
        }
        int rows = matrix.length, columns = matrix[0].length;
        boolean[][] visited = new boolean[rows][columns];
        int total = rows * columns;
        int row = 0, column = 0;
        int[][] directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        int directionIndex = 0;
        for (int i = 0; i < total; i++) {
            order.add(matrix[row][column]);
            visited[row][column] = true;
            int nextRow = row + directions[directionIndex][0], nextColumn = column + directions[directionIndex][1];
            if (nextRow < 0 || nextRow >= rows || nextColumn < 0 || nextColumn >= columns || visited[nextRow][nextColumn]) {
                directionIndex = (directionIndex + 1) % 4;
            }
            row += directions[directionIndex][0];
            column += directions[directionIndex][1];
        }
        return order;
    }
}
```

### 热评：

```
def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        res = []
        while matrix:
            # 削头（第一层）
            res += matrix.pop(0)
            # 将剩下的逆时针转九十度，等待下次被削
            matrix = list(zip(*matrix))[::-1]
        return res
```

```
//将已经走过的地方置 NONE，然后拐弯的时候判断一下是不是已经走过了，如果走过了就计算一下新的方向：
//
r, i, j, di, dj = [], 0, 0, 0, 1
if matrix != []:
    for _ in range(len(matrix) * len(matrix[0])):
        r.append(matrix[i][j])
        matrix[i][j] = NONE
        if matrix[(i + di) % len(matrix)][(j + dj) % len(matrix[0])] == 0:
            di, dj = dj, -di
        i += di
        j += dj
return r
```

