### 题目：

给你二叉树的根节点 root 和一个表示目标和的整数 targetSum ，判断该树中是否存在 根节点到叶子节点 的路径，这条路径上所有节点值相加等于目标和 targetSum 。

叶子节点 是指没有子节点的节点。

### 难度：

简单。

### 示例：

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
输出：true
```

### 解题思路：

```c++
    深度优先遍历的同时，记录已经累加的值和总和。
    判断是否为空指针，如果是说明其是某个非叶节点的某个节点，直接跳过循环。
    判断是否为叶子节点，如果是计算累加和并比较。
    如果都不是，计算累计和，存储其左右指针。
    关键在于如何弹出以及何时弹出父节点的值。

    1.可以使用递归不断遍历所有可能的路径。

    2.可以使用迭代来实现。
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
    //递归函数
    bool findPath(TreeNode* root, int targetSum, int sum){
        //判断是否为空指针
        if( root == nullptr ){
            return false;
        }
        //判断是否为叶节点
        if(root->left == nullptr && root->right == nullptr){
            if(targetSum - sum == root->val){
                return true;
            }else{
                return false;
            }
        }
        if(findPath(root->left,targetSum,sum+root->val)){
            return true;
        }
        if(findPath(root->right,targetSum,sum+root->val)){
            return true;
        }
        return false;
    }
    bool hasPathSum(TreeNode* root, int targetSum) {
        return findPath(root, targetSum, 0);
    }
};
```

### 知识点：

类似于二叉树的 遍历，既可以使用递归，也可以使用迭代实现。

### 官方代码：

```c++
class Solution {
public:
    bool hasPathSum(TreeNode *root, int sum) {
        if (root == nullptr) {
            return false;
        }
        queue<TreeNode *> que_node;
        queue<int> que_val;
        que_node.push(root);
        que_val.push(root->val);
        while (!que_node.empty()) {
            TreeNode *now = que_node.front();
            int temp = que_val.front();
            que_node.pop();
            que_val.pop();
            if (now->left == nullptr && now->right == nullptr) {
                if (temp == sum) {
                    return true;
                }
                continue;
            }
            if (now->left != nullptr) {
                que_node.push(now->left);
                que_val.push(now->left->val + temp);
            }
            if (now->right != nullptr) {
                que_node.push(now->right);
                que_val.push(now->right->val + temp);
            }
        }
        return false;
    }
};
```

### 热评：

```
一打卡，就是一天
```

