### 题目：

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

>   一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1 。

### 难度：

中等。

### 示例：

```
输入：root = [3,9,20,null,null,15,7]
输出：true
```

### 解题思路：

```c++
求出左子树深度和右子树深度，比较两者之差
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
    bool isBalanced(TreeNode* root) {
        //排除特殊情况
        if(root == nullptr){
            return true;
        }
        //获取左右子树深度
        int leftDepth = 0, rightDepth = 0;
        leftDepth = calcDepth(root->left);
        rightDepth = calcDepth(root->right);
        //cout << "leftDepth: " << leftDepth << "  rightDepth:  " << rightDepth << endl;
        if(leftDepth == -1 || rightDepth == -1){
            return false;
        }
        if( abs(leftDepth - rightDepth) > 1){
            return false;
        }

        return true;
    }
private:
    //不满足平衡二叉树，返回-1.否则返回子树的最大深度。
    int calcDepth(TreeNode* root){
        if(root == nullptr) return 0;
        //比较左右子树深度
        int leftDepth = 0, rightDepth = 0;
        leftDepth = calcDepth(root->left);
        rightDepth = calcDepth(root->right);
        if( leftDepth == -1 || rightDepth == -1){
            return -1;
        }
        if(abs(leftDepth - rightDepth) > 1){
            return -1;
        }
        return max(leftDepth, rightDepth) + 1;
    }
};

```

### 知识点：

关键在于如果子树已经不平衡了及时返回。

### 官方代码：

```c++
class Solution {
public:
    int height(TreeNode* root) {
        if (root == NULL) {
            return 0;
        } else {
            return max(height(root->left), height(root->right)) + 1;
        }
    }

    bool isBalanced(TreeNode* root) {
        if (root == NULL) {
            return true;
        } else {
            return abs(height(root->left) - height(root->right)) <= 1 && isBalanced(root->left) && isBalanced(root->right);
        }
    }
};

```

### 热评：

```
自底向上的递归就是及时止损 自顶向下的递归就是不管三七二十一先把所有的高度算一遍
```

