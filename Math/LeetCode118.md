### 题目：

给定一个非负整数 *`numRows`，*生成「杨辉三角」的前 *`numRows`* 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

### 难度：

简单。

### 示例：

```
输入: numRows = 5
输出: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

### 解题思路：

```
    按杨辉三角的计算公式计算每一层。
```

### 代码实现：

```c++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        //返回结果和上一层。
        vector<vector<int>> res;
        vector<int> temp, temp1;
        //第一层。
        temp.push_back(1);
        res.push_back(temp);
        if(numRows == 1){            
            return res;
        }
        //递增
        while(numRows > 1 ){
            temp1.clear();
            temp1.push_back(1);
            for(int i=0; i < temp.size() - 1; i++){
                temp1.push_back(temp[i] + temp[i+1]);
            }
            temp1.push_back(1);
            temp.clear();
            temp = temp1;
            res.push_back(temp);
            numRows--;
        }
        return res;
    }
};

```

### 知识点：

杨辉三角通过递推得到。

### 官方代码：

```
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> ret(numRows);
        for (int i = 0; i < numRows; ++i) {
            ret[i].resize(i + 1);
            ret[i][0] = ret[i][i] = 1;
            for (int j = 1; j < i; ++j) {
                ret[i][j] = ret[i - 1][j] + ret[i - 1][j - 1];
            }
        }
        return ret;
    }
};
```

### 热评：

```
简单题我重拳出击，中等题我努力思考，困难题我复制粘贴
```

