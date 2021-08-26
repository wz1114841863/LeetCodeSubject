### 题目：

给定一个二叉树的 **根节点** `root`，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

### 难度：

中等。

### 示例：

```
输入: [1,2,3,null,5,null,4]
输出: [1,3,4]
```

### 解题思路：

```
层次遍历，返回每层遍历的最后一个值.
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
    vector<int> rightSideView(TreeNode* root) {
        //返回结果
        vector<int> res;
        //排除特殊情况
        if(root == nullptr) return res;
        //根节点为第一层，肯定能看到
        res.push_back(root->val);
        //层次遍历，将每次层次遍历的最后一个数放入res的末尾。
        vector<TreeNode* > temp;
        if(root->left != nullptr) temp.push_back(root->left);
        if(root->right != nullptr) temp.push_back(root->right);
        levelTraversal(res, temp);

        return res;
    }
private:
    void levelTraversal(vector<int> &res, vector<TreeNode*> &temp){
        //边界条件
        if(temp.empty()) return ;
        //中间变量
        vector<TreeNode*> temp1 = temp;
        temp.clear();
        int len = temp1.size();
        for(int i = 0; i < len; i++){
            if(temp1[i]->left != nullptr) temp.push_back(temp1[i]->left);
            if(temp1[i]->right != nullptr) temp.push_back(temp1[i]->right);
            if(i == len - 1){
                res.push_back(temp1[i]->val);
            }
        }
        levelTraversal(res, temp);
        return; 
    }
};
```

### 知识点：

理解题意就能很快想到。

### 官方代码：

```
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        unordered_map<int, int> rightmostValueAtDepth;
        int max_depth = -1;

        stack<TreeNode*> nodeStack;
        stack<int> depthStack;
        nodeStack.push(root);
        depthStack.push(0);

        while (!nodeStack.empty()) {
            TreeNode* node = nodeStack.top();nodeStack.pop();
            int depth = depthStack.top();depthStack.pop();

            if (node != NULL) {
            	// 维护二叉树的最大深度
                max_depth = max(max_depth, depth);

                // 如果不存在对应深度的节点我们才插入
                if (rightmostValueAtDepth.find(depth) == rightmostValueAtDepth.end()) {
                    rightmostValueAtDepth[depth] =  node -> val;
                }

                nodeStack.push(node -> left);
                nodeStack.push(node -> right);
                depthStack.push(depth + 1);
                depthStack.push(depth + 1);
            }
        }

        vector<int> rightView;
        for (int depth = 0; depth <= max_depth; ++depth) {
            rightView.push_back(rightmostValueAtDepth[depth]);
        }

        return rightView;
    }
};
```

### 热评：

```
今天的官解很不错，ctrl+c / ctrl+v 了。
```

