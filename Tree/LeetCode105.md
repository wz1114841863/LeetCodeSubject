### 题目：

给定一棵树的前序遍历 `preorder` 与中序遍历 `inorder`。请构造二叉树并返回其根节点。

### 难度：

中等。

### 示例：

```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```

### 解题思路：

```c++
    1.前序遍历的第一个元素就是根节点。
    找到根节点在中序遍历中的位置，左边就是左子树，右边就是右子树。
    左子树包含的个数为[0,root) l1 = rootLoc - 1;
    右子树包含的个数为(root, len-1] r1 = len - rootLoc;
    2.利用递归，不断处理其左子树和右子树

    改进：
        通过添加一个哈希表能够很快的定位到根节点的位置，同时也不需要使用find函数和迭代器。
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
    //参数： preorder， inorder, 子树位于preorder 和 inorder 中的位置。 [left, right]
    TreeNode* buildChildTree(const vector<int>& preorder, const vector<int>& inorder, int preorder_left, int preorder_right, int inorder_left, int inorder_right){
        //边界条件
        if(preorder_left > preorder_right){
            return nullptr;
        }
        // 前序遍历中的第一个节点就是根节点
        int preorder_root = preorder_left;
        // 在中序遍历中定位根节点
        int inorder_root = index[preorder[preorder_root]]; 
        // 建立根节点
        TreeNode* root = new TreeNode(preorder[preorder_root]);
        // 得到左子树中的节点数目
        int size_left_subtree = inorder_root - inorder_left;
        // 递归地构造左子树，并连接到根节点
        // 先序遍历中「从 左边界+1 开始的 size_left_subtree」个元素就对应了中序遍历中「从 左边界 开始到 根节点定位-1」的元素
        root->left = buildChildTree(preorder, inorder, preorder_left + 1, preorder_left + size_left_subtree, inorder_left, inorder_root - 1);
        // 递归地构造右子树，并连接到根节点
        // 先序遍历中「从 左边界+1+左子树节点数目 开始到 右边界」的元素就对应了中序遍历中「从 根节点定位+1 到 右边界」的元素
        root->right = buildChildTree(preorder, inorder, preorder_left + size_left_subtree + 1, preorder_right, inorder_root + 1, inorder_right);
        return root;        
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        //获取inorder 长度，同时 记录每个值的位置
        int len = inorder.size();
        //存储值到哈希表中
        for(int i=0; i<len; i++){
            index[inorder[i]] = i;
        }
        //调用递归
        return buildChildTree(preorder, inorder, 0, len - 1, 0, len - 1); 
        
    }
private:
    //定义一个哈希树，用来记录每一次头节点所在的位置。
    unordered_map<int, int> index;
};
```

### 知识点：

除了使用递归外，官方还提供了一种迭代解法

### 官方代码：

```c++
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if (!preorder.size()) {
            return nullptr;
        }
        TreeNode* root = new TreeNode(preorder[0]);
        stack<TreeNode*> stk;
        stk.push(root);
        int inorderIndex = 0;
        for (int i = 1; i < preorder.size(); ++i) {
            int preorderVal = preorder[i];
            TreeNode* node = stk.top();
            if (node->val != inorder[inorderIndex]) {
                node->left = new TreeNode(preorderVal);
                stk.push(node->left);
            }
            else {
                while (!stk.empty() && stk.top()->val == inorder[inorderIndex]) {
                    node = stk.top();
                    stk.pop();
                    ++inorderIndex;
                }
                node->right = new TreeNode(preorderVal);
                stk.push(node->right);
            }
        }
        return root;
    }
};
```

### 热评：

```
preorder第一个元素为root，在inorder里面找到root，在它之前的为左子树（长l1），之后为右子树（长l2）。preorder[1]到preorder[l1]为左子树,之后为右子树，分别递归。
```

