### 题目：

给你二叉树的根节点 `root` ，返回它节点值的 **前序** 遍历。

### 难度：

简单。

### 示例：

```
输入：root = [1,null,2,3]
输出：[1,2,3]
```

### 解题思路：

```
有递归和遍历两种方法。
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
/*
    递归。
*/
/*
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
 
*/
//遍历
//利用栈实现递归，出栈时，先保存当前值至res，再入栈其右指针，左指针。
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        //返回结果
        vector<int> res;
        //利用栈来存储每个节点
        stack<TreeNode *> s1;
        TreeNode* temp;
        //先入栈根节点
        if(root == nullptr){
            return res;
        }else{
            s1.push(root);
        }
        while(!s1.empty()){
            temp = s1.top();
            s1.pop();
            res.push_back(temp->val);
            if(temp->right != nullptr) s1.push(temp->right);
            if(temp->left != nullptr) s1.push(temp->left);

        }
        return res;       
    }
};
```

### 知识点：

这次的迭代代码跟之前先序遍历的相比出乎意料的简洁。

### 官方代码：

```
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        if (root == nullptr) {
            return res;
        }

        stack<TreeNode*> stk;
        TreeNode* node = root;
        while (!stk.empty() || node != nullptr) {
            while (node != nullptr) {
                res.emplace_back(node->val);
                stk.emplace(node);
                node = node->left;
            }
            node = stk.top();
            stk.pop();
            node = node->right;
        }
        return res;
    }
};
```

### 热评：

```
会迭代时看递归头疼，会递归后看迭代头疼
```

