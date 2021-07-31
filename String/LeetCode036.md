### 题目：

请你判断一个 9x9 的数独是否有效。只需要 根据以下规则 ，验证已经填入的数字是否有效即可。

数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。（请参考示例图）
数独部分空格内已填入了数字，空白格用 '.' 表示。

注意：

一个有效的数独（部分已被填充）不一定是可解的。
只需要根据以上规则，验证已经填入的数字是否有效即可。

### 难度：

中等。

### 示例：

```
输入：board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
输出：true
```

### 解题思路：

```
初始想法：
    利用set来存储每行，每列，每个区域的数字。
    逐个遍历。
    判断当前元素是否为数字
        如果是判断所属行列是否唯一，所在九宫格是否唯一。
            如果唯一，遍历下一个。
            不唯一直接返回false。
        如果不是（即为'.'，查找下一个字符。
```

### 代码实现：

```c++
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        //创建set，存储结果。
        //类型设为char。
        //row, col, loc
        set<char> row[9];
        set<char> col[9];
        //分为九个区域
        //根据i / 3 与 j / 3
        set<char> loc[9];
        //区域号。loa_cal = i / 3 * 3 + j / 3;
        int loc_cal; 
        //临时字符
        char temp;
        //双重遍历
        for(int i=0; i<9; i++){
            for( int j=0; j<9; j++){
                //判断当前元素
                temp = board[i][j];
                if( temp != '.'){
                    loc_cal = i / 3 * 3 + j / 3;
                    if((row[i].count(temp)) || (col[j].count(temp)) || (loc[loc_cal].count(temp)) ){
                        return false;
                    }
                    row[i].insert(temp);
                    col[j].insert(temp);
                    loc[loc_cal].insert(temp);
                }
            }
        }
        //没有返回，说明符合条件
        return true;
    }
}; 
```

### 知识点：

感觉就是暴力AC,但是发现官方也没有更好的思路。

### 官方代码：

```
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        vector<vector<int>> row (9, vector<int>(9,0));
        vector<vector<int>> col (9, vector<int>(9,0));
        vector<vector<int>> box (9, vector<int>(9,0));

        for(int i=0;i<9;i++){
            for(int j=0;j<9;j++){
                if(board[i][j] == '.'){
                    continue;
                }
                int val = board[i][j] - '1';
                int box_index = (i/3) * 3 + j/3;
                if(row[i][val] == 0 && col[j][val] == 0 && box[box_index][val] == 0){
                    row[i][val] = 1;
                    col[j][val] = 1;
                    box[box_index][val] = 1;
                }
                else{
                    return false;
                }
            }
        }
        return true;
    }
};
```

### 热评：

```
9个数用什么hashmap,直接数组解决。
```

