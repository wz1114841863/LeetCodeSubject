### 题目：

给你一个二叉树，请你返回其按 **层序遍历** 得到的节点值。 （即逐层地，从左到右访问所有节点）。

### 难度：

中等。

### 示例：

```
二叉树：[3,9,20,null,null,15,7]
[
  [3],
  [9,20],
  [15,7]
]
```

### 解题思路：

```
    广度优先搜索。
    存储值的同时存储左右指针。
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
    vector<vector<int>> levelOrder(TreeNode* root) {
        //返回结果
        vector<vector<int>> res;
        //排除特殊情况
        if(root == nullptr){
            return res;
        }
        //存储指针和向量
        vector<TreeNode *> node1, node2;
        vector<int> temp;
        //node1存储上一层的所有指针
        node1.push_back(root);
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
            node1 = node2;
            res.push_back(temp);
        }

        return res;
    }
};
```

### 知识点：

广度优先搜索。

### 官方代码：

```
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector <vector <int>> ret;
        if (!root) {
            return ret;
        }

        queue <TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            int currentLevelSize = q.size();
            ret.push_back(vector <int> ());
            for (int i = 1; i <= currentLevelSize; ++i) {
                auto node = q.front(); q.pop();
                ret.back().push_back(node->val);
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
        }
        
        return ret;
    }
};

```

### 热评：

```
第一次直接在提交记录里面直接手敲出来，被自己感动了
```

