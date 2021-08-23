### 题目：

给定一个二叉树，返回其节点值的锯齿形层序遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

### 难度：

中等。

### 示例：

```
[3,9,20,null,null,15,7]
[
  [3],
  [20,9],
  [15,7]
]
```

### 解题思路：

```
 在上一题的代码上进行修改。加个标志位。
 翻转。
```

### 代码实现：

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        //返回结果
        vector<vector<int>> res;
        //排除特殊情况
        if(root == nullptr){
            return res;
        }
        //存储指针和向量
        vector<TreeNode *> node1, node2;
        vector<int> temp, temp1;
        //node1存储上一层的所有指针
        node1.push_back(root);
        //标志位： 0 从左到右   1 从右到左。
        int flag = 0;
        while(!node1.empty()){
            temp.clear();
            node2.clear();
            int len = node1.size();
            for(int i=0; i<len; i++){
                temp.push_back(node1[i]->val);
                if(node1[i]->left != nullptr){
                    node2.push_back(node1[i]->left);
                }
                if(node1[i]->right != nullptr){
                    node2.push_back(node1[i]->right);
                }
            }
            if(flag == 1){
                for(int i=temp.size()-1; i>=0; i--){
                    temp1.push_back(temp[i]);
                }
                temp = temp1;
                temp1.clear();
            }
            node1 = node2;
            res.push_back(temp);
            (flag == 0) ? flag = 1 : flag = 0;
        }

        return res;
    }
};
```

### 知识点：

翻转有点过于浪费时间。可以进一步优化。

### 官方代码：

```
官方解答：
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if (!root) {
            return ans;
        }

        queue<TreeNode*> nodeQueue;
        nodeQueue.push(root);
        bool isOrderLeft = true;

        while (!nodeQueue.empty()) {
            deque<int> levelList;
            int size = nodeQueue.size();
            for (int i = 0; i < size; ++i) {
                auto node = nodeQueue.front();
                nodeQueue.pop();
                if (isOrderLeft) {
                    levelList.push_back(node->val);
                } else {
                    levelList.push_front(node->val);
                }
                if (node->left) {
                    nodeQueue.push(node->left);
                }
                if (node->right) {
                    nodeQueue.push(node->right);
                }
            }
            ans.emplace_back(vector<int>{levelList.begin(), levelList.end()});
            isOrderLeft = !isOrderLeft;
        }

        return ans;
    }
};
```

### 热评：

```
直接层序遍历加结果奇数下标倒序完事
```

