### 题目：

给定二叉搜索树的根结点 `root`，返回值位于范围 *`[low, high]`* 之间的所有结点的值的和。

### 难度：

简单。

### 示例：

```
输入：root = [10,5,15,3,7,null,18], low = 7, high = 15
输出：32
```

### 解题思路：

```c++
初始想法：
    利用二叉树的中序遍历为升序这一特性，中序遍历过程中，判断当前值是否满足条件。
        1.小于low,接着遍历
        2.位于[low, high]范围内，累加和。
剪枝：
    根据root->val的值，判断是否对左右子树进行遍历。
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
    int low, high;
    void inorderTraverse(TreeNode* root,  int& sum){
        //边界条件
        if(root == nullptr) return ;
        //递归遍历
        inorderTraverse(root->left, sum);
        if(root->val >= low && root->val <= high){
             sum += root->val; 
        }        
        inorderTraverse(root->right, sum);

        return ;
    }
    int rangeSumBST(TreeNode* root, int low, int high){
        this->low = low;
        this->high = high;
        //返回结果
        int sum = 0;
        inorderTraverse(root, sum);
        return sum;
    }
};
```

### 知识点：

二叉搜索树的特性使得它有利于进行中序遍历。

### 官方代码：

```c++
class Solution {
public:
    int rangeSumBST(TreeNode *root, int low, int high) {
        if (root == nullptr) {
            return 0;
        }
        if (root->val > high) {
            return rangeSumBST(root->left, low, high);
        }
        if (root->val < low) {
            return rangeSumBST(root->right, low, high);
        }
        return root->val + rangeSumBST(root->left, low, high) + rangeSumBST(root->right, low, high);
    }
};
```

### 热评：

```
这题不是有手就行。
```

