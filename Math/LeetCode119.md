### 题目：

给定一个非负索引 `rowIndex`，返回「杨辉三角」的第 `rowIndex` 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

### 难度：

简单。

### 示例：

```
输入: rowIndex = 3
输出: [1,3,3,1]
```

### 解题思路：

```
按杨辉三角的计算公式计算每一层。
```

### 代码实现：

```c++
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        //返回结果和上一层。
        vector<int> res, temp;
        //第零层。
        temp.push_back(1);
        if(rowIndex == 0){
            return temp;
        }
        //递增
        while(rowIndex > 0 ){
            res.clear();
            res.push_back(1);
            for(int i=0; i < temp.size() - 1; i++){
                res.push_back(temp[i] + temp[i+1]);
            }
            res.push_back(1);
            temp.clear();
            temp = res;
            rowIndex--;
        }
        return res;
    }
};
```

### 知识点：

除了根据递推还可以利用数学公式来计算。

### 官方代码：

```
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<int> row(rowIndex + 1);
        row[0] = 1;
        for (int i = 1; i <= rowIndex; ++i) {
            for (int j = i; j > 0; --j) {
                row[j] += row[j - 1];
            }
        }
        return row;
    }
};
```

### 热评：

```
希望大家今年纠结于去腾讯还是阿里
```

```
冒泡排序，选择排序，插入排序，快速排序，堆排序，归并排序，希尔排序，桶排序，基数排序新年帮您排忧解难。
有向图，无向图，有环图，无环图，完全图，稠密图，稀疏图，拓扑图祝您新年宏图大展。
最长路，最短路，单源路径，所有节点对路径祝您新年路路通畅。
二叉树，红黑树，van Emde Boas树，最小生成树祝您新年好运枝繁叶茂。
最大流，网络流，标准输入流，标准输出流，文件输入流，文件输出流祝您新年顺顺流流。
线性动规，区间动规，坐标动规，背包动规，树型动归为您的新年规划精彩。
散列表，哈希表，邻接表，双向链表，循环链表帮您在新年表达喜悦。 O(1), O(log n), O(n), O(nlog n), O(n^2), O(n^3), O(2^n), O(n!)祝大家新年渐进步步高。
```

