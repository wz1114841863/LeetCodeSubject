### 题目：

给定一个 m x n 二维字符网格 board 和一个字符串单词 word 。如果 word 存在于网格中，返回 true ；否则，返回 false 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

### 难度：

中等。

### 示例：

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```

### 解题思路：

```
初始想法：
    先查找第一个字母可能在的位置，利用栈1存贮。
    然后遍历栈1，从初始字母位置四周查找下一个字母
        如果查找到，入栈2，递归查找下一个
        如果未查找到，出栈，进行下一个点的尝试。
    为了避免重复使用元素，可以利用(m, n)的数组来保存已用过的位置
```

### 代码实现：

```c++
class Solution {
public:
    int m, n, len;
    bool exist(vector<vector<char>>& board, string word) {
        //获取m，n
        m = board.size();
        n = board[0].size();
        //测试代码
        /*
        cout << "m: " << m << endl;
        cout << "n: " << n << endl;
        */
        //获取word的长度
        len = word.size();
        //cout << "len: " << len << endl;
        //排除特殊情况
        if(m * n < len){
            //向量长度小于字符串长度，不可能匹配
            return false;
        }
        //创建向量
        vector<vector<int>> arrayChar(m);
        for(int i=0; i<m; i++){
            arrayChar[i].resize(n);
        }
        //首先查找首字母可能出现的位置
        vector<pair<int, int>> stack1;
        pair<int, int> loc;
        char tempChar = word[0];
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                if(board[i][j] == tempChar){
                    loc.first = i;
                    loc.second = j;
                    stack1.push_back(loc);
                }
            }
        }
        //测试代码，输出stack1
        //遍历查找
        bool res;
        while(!stack1.empty()){
            loc = stack1.back();
            arrayChar[loc.first][loc.second] = 1;
            res = findWord(board, arrayChar, loc, word, 1);
            if(res){
                break;
            }
            arrayChar[loc.first][loc.second] = 0;
            stack1.pop_back();
        }
        return res;
    }
private:    
    bool findWord(vector<vector<char>>& board, vector<vector<int>> &arrayChar, 
            pair<int, int> loc, string &word, int num){
        //cout << num << endl; 
        //边界条件
        if(num == len){
            return true;
        }
        //查找四周
        char tempChar = word[num];
        int x = loc.first;
        int y = loc.second;
        bool res;
        //上
        if(x-1 >= 0 && arrayChar[x-1][y] != 1 && board[x-1][y] == tempChar){
            //位置合法且字符相等。
            arrayChar[x-1][y] = 1;
            loc.first = x-1;
            loc.second = y;
            res = findWord(board, arrayChar, loc, word, num+1);
            if(res){
                return res;
            }
            arrayChar[x-1][y] = 0;
        }
        //左
        if(y - 1 >= 0 && arrayChar[x][y-1] != 1 && board[x][y-1] == tempChar){
            //位置合法且字符相等。
            arrayChar[x][y-1] = 1;
            loc.first = x;
            loc.second = y-1;
            res = findWord(board, arrayChar, loc, word, num+1);
            if(res){
                return res;
            }  
            arrayChar[x][y-1] = 0;          
        }
        //下
        if(x + 1 < m && arrayChar[x+1][y] != 1 && board[x+1][y] == tempChar){
            //位置合法且字符相等。
            arrayChar[x+1][y] = 1;
            loc.first = x+1;
            loc.second = y;
            res = findWord(board, arrayChar, loc, word, num+1);
            if(res){
                return res;
            } 
            arrayChar[x+1][y] = 0;  
        }
        //右
        if(y + 1 < n && arrayChar[x][y+1] != 1 && board[x][y+1] == tempChar){
            //位置合法且字符相等。
            arrayChar[x][y+1] = 1;
            loc.first = x;
            loc.second = y+1;
            res = findWord(board, arrayChar, loc, word, num+1);
            if(res){
                return res;
            } 
            arrayChar[x][y+1] = 0;  
        } 
        //loc无效，返回。

        return false;       
    }
};
```

### 知识点：

代码还是过于臃肿，烦啊。

### 官方代码：

```
class Solution {
public:
    bool check(vector<vector<char>>& board, vector<vector<int>>& visited, int i, int j, string& s, int k) {
        if (board[i][j] != s[k]) {
            return false;
        } else if (k == s.length() - 1) {
            return true;
        }
        visited[i][j] = true;
        vector<pair<int, int>> directions{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        bool result = false;
        for (const auto& dir: directions) {
            int newi = i + dir.first, newj = j + dir.second;
            if (newi >= 0 && newi < board.size() && newj >= 0 && newj < board[0].size()) {
                if (!visited[newi][newj]) {
                    bool flag = check(board, visited, newi, newj, s, k + 1);
                    if (flag) {
                        result = true;
                        break;
                    }
                }
            }
        }
        visited[i][j] = false;
        return result;
    }

    bool exist(vector<vector<char>>& board, string word) {
        int h = board.size(), w = board[0].size();
        vector<vector<int>> visited(h, vector<int>(w));
        for (int i = 0; i < h; i++) {
            for (int j = 0; j < w; j++) {
                bool flag = check(board, visited, i, j, word, 0);
                if (flag) {
                    return true;
                }
            }
        }
        return false;
    }
};
```

### 热评：

```
C++在做递归回溯算法相关题目时，递归函数形参传值和传引用运行速度有很大的差异。这是我这道题dfs函数的声明，主要区别是visited和word，一个是传值，一个是传引用。前者执行超时，后者在本题是32ms.

个人理解为传值时每次递归调用都要在内存中新建立一个vector 来保存visit传入的值，但是传引用直接在visited原始位置操作，不需要进行新建变量与赋值，节省了代码运行的空间与时间开销。

```

```
void dfs(vector<vector<char>>& board,vector<vector<int>>visited,int x,int y,int n,string word,bool& flag)
void dfs(vector<vector<char>>& board,vector<vector<int>>& visited,int x,int y,int n,string& word,bool& flag)
```

