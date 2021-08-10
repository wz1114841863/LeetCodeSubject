### 题目：

给你一个字符串 path ，表示指向某一文件或目录的 Unix 风格 绝对路径 （以 '/' 开头），请你将其转化为更加简洁的规范路径。

在 Unix 风格的文件系统中，一个点（.）表示当前目录本身；此外，两个点 （..） 表示将目录切换到上一级（指向父目录）；两者都可以是复杂相对路径的组成部分。任意多个连续的斜杠（即，'//'）都被视为单个斜杠 '/' 。 对于此问题，任何其他格式的点（例如，'...'）均被视为文件/目录名称。

请注意，返回的 规范路径 必须遵循下述格式：

始终以斜杠 '/' 开头。
两个目录名之间必须只有一个斜杠 '/' 。
最后一个目录名（如果存在）不能 以 '/' 结尾。
此外，路径仅包含从根目录到目标文件或目录的路径上的目录（即，不含 '.' 或 '..'）。
返回简化后得到的 规范路径 。

### 难度：

中等。

### 示例：

```
输入：path = "/../"
输出："/"
解释：从根目录向上一级是不可行的，因为根目录是你可以到达的最高级。
```

### 解题思路：

```
需要先考虑多个斜杠问题
用 '/'分割字符串.
    文件夹名入栈
    '.' 不做操作
    '..' 出栈栈顶元素

```

### 代码实现：

```c++
class Solution {
public:
    string simplifyPath(string path){
        //存储文件名
        stack<string> foldersName;
        string name;
        //末尾添加'/'
        path = path + '/';
        //根据查找到的'/'字符位置分割字符
        int pos;
        pos = path .find("/");
        while(pos != path.npos){
            if(pos == 0){
                path.erase(pos, 1);
            }else{
                name = path.substr(0, pos);
                if(name != "." && name != ".."){
                    foldersName.push(name);
                }else if(name == ".." && !foldersName.empty()){
                    foldersName.pop();
                }
                path.erase(0, pos+1);
            }
            pos = path .find("/");
            //cout << path << endl;
        }
        if(foldersName.empty()){
            return  (string)"/";
        }
        //打印合格字符串
        string res; 
        while(!foldersName.empty()){
            res = "/" + foldersName.top() + res;
            foldersName.pop();
        }
        return res;
    }
};
```

### 知识点：

这里使用了堆栈，看别人的解答也可以用向量和双端队列实现

### 优秀解答：

```
class Solution {
    public String simplifyPath(String path) {
        // 双端队列
        Deque<String> queue = new LinkedList<>();
        // 分割字符
        String[] res = path.split("/");
        for(int i = 0; i < res.length; i++){
            String s = res[i];
            if(s.equals(".") || s.equals("")) continue;
            else if (s.equals("..")){
                if(!queue.isEmpty()){
                    queue.pollLast();
                }
            }else{
                queue.offer(s);
            }
        }
        // 拼接
        StringBuilder sb = new StringBuilder("/");
        while(!queue.isEmpty()){
            sb.append(queue.poll());
            if(!queue.isEmpty()){
                sb.append("/");
            }
        }
        // 判空
        return sb.toString().equals("") ? "/" : sb.toString();
    }
}
```

### 热评：

```
"/..."应该等于多少？
```

