### 题目：

根据一棵树的中序遍历与后序遍历构造二叉树。

**注意:**
你可以假设树中没有重复的元素。

### 难度：

中等。

### 示例：

```
中序遍历 inorder = [9,3,15,20,7]
后序遍历 postorder = [9,15,7,20,3]
返回如下的二叉树：
    3
   / \
  9  20
    /  \
   15   7
```

### 解题思路：

```c++
    与上一题唯一的不同在于后序遍历中，根节点位于后序遍历的末尾。
    修改上一题的代码。
    同时要注意递归时的范围。
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
    //参数： postorder， inorder, 子树位于preorder 和 inorder 中的位置。 [left, right]
    TreeNode* buildChildTree(const vector<int>& inorder, const vector<int>& postorder, int postorder_left, int postorder_right, int inorder_left, int inorder_right){
        //边界条件
        if(postorder_left > postorder_right){
            return nullptr;
        }
        //前序遍历中的最后一个节点就是根节点
        int postorder_root = postorder_right;
        // 在中序遍历中定位根节点
        int inorder_root = index[postorder[postorder_root]]; 
        // 建立根节点
        TreeNode* root = new TreeNode(postorder[postorder_root]);
        // 得到左子树中的节点数目
        int size_left_subtree = inorder_root - inorder_left;
        // 递归地构造左子树，并连接到根节点
        // 先序遍历中「从 左边界+1 开始的 size_left_subtree」个元素就对应了后序遍历中「从 左边界 开始到 根节点定位-1」的元素
        root->left = buildChildTree(inorder, postorder,  postorder_left, postorder_left + size_left_subtree - 1, inorder_left, inorder_root - 1);
        // 递归地构造右子树，并连接到根节点
        // 先序遍历中「从 左边界+1+左子树节点数目 开始到 右边界」的元素就对应了后序遍历中「从 根节点定位+1 到 右边界」的元素
        root->right = buildChildTree( inorder, postorder, postorder_left + size_left_subtree, postorder_right - 1, inorder_root + 1, inorder_right);
        return root;        
    }

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        //获取inorder 长度，同时 记录每个值的位置
        int len = inorder.size();
        //存储值到哈希表中
        for(int i=0; i<len; i++){
            index[inorder[i]] = i;
        }
        //调用递归
        return buildChildTree(inorder, postorder, 0, len - 1, 0, len - 1); 
        
    }
private:
    //定义一个哈希树，用来记录每一次头节点所在的位置。
    unordered_map<int, int> index;
};
```

### 知识点：



### 官方代码：

```c++
class Solution {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (postorder.size() == 0) {
            return nullptr;
        }
        auto root = new TreeNode(postorder[postorder.size() - 1]);
        auto s = stack<TreeNode*>();
        s.push(root);
        int inorderIndex = inorder.size() - 1;
        for (int i = int(postorder.size()) - 2; i >= 0; i--) {
            int postorderVal = postorder[i];
            auto node = s.top();
            if (node->val != inorder[inorderIndex]) {
                node->right = new TreeNode(postorderVal);
                s.push(node->right);
            } else {
                while (!s.empty() && s.top()->val == inorder[inorderIndex]) {
                    node = s.top();
                    s.pop();
                    inorderIndex--;
                }
                node->left = new TreeNode(postorderVal);
                s.push(node->left);
            }
        }
        return root;
    }
};

```

### 热评：

```
做得出来递归，想不明白迭代。
```

