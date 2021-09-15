### 题目：

给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 输入数据 保证 ，新值和原始二叉搜索树中的任意节点值都不同。

注意，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回 任意有效的结果 。

### 难度：

中等。

### 示例：

```
输入：root = [4,2,7,1,3], val = 5
输出：[4,2,7,1,3
```

### 解题思路：

```c++
    比较节点值与当前根节点值：
    1.大于根节点：
        判断其右节点是否为空：
            不为空，递归其右子树
            为空，插入节点
    2.小于根节点：
        判断其左节点是否为空：
            不为空，递归其左子树
            为空，插入节点
```

### 代码实现：

```c++

```

### 知识点：

模仿二叉搜索树的建立即可。

### 官方代码：

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
    bool insertNode(TreeNode* root, int val){
        if(root == nullptr) return false;
        if(root->val < val){
            if(root->right == nullptr){
                TreeNode* node = new TreeNode(val);
                root->right = node;
                return true;
            }else{
                if(insertNode(root->right, val)) return true;
            }
        }else{
            if(root->left == nullptr){
                TreeNode* node = new TreeNode(val);
                root->left = node;
                return true;
            }else{
                if(insertNode(root->left, val)) return true;
            }
        }   

        return false;
    }
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        //排除特殊情况
        if(root == nullptr) return new TreeNode(val);
        insertNode(root, val);
        return root;
    }
};
```

### 热评：

```
做过的最简单的中等题。
```

