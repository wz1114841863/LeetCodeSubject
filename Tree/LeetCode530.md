### 题目：

给你一个二叉搜索树的根节点 `root` ，返回 **树中任意两不同节点值之间的最小差值** 。

### 难度：

简单

### 示例：

```
输入：root = [4,2,6,1,3]
输出：1
```

### 解题思路：

```c++
错误想法：
    只用考虑根节点与相领子节点的差距。
错误原因：    
    左子树的右节点与根节点差值可能更小。

利用中序遍历，实现树节点的升序排列。然后比较相邻值的差值。
中序遍历使用了递归，使用向量存储了所有节点的值。
时间复杂度：O(n) 主要用于递归和遍历向量
空间复杂度O(n): 递归树的深度，最坏情况为链式时，复杂度为O(n)，向量的大小为：O(N)

为了提高效率可以使用：
    1.非递归的方式遍历二叉树
    2.不用记录所有节点的值，而是遍历同时进行对比。
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
class Solution {
public:
    void inorderTraverse(TreeNode* root, vector<int> &res){
        //边界条件
        if(root->left == nullptr && root->right == nullptr){
            res.emplace_back(root->val);
            return ;
        }

        if(root->left != nullptr) inorderTraverse(root->left, res);
        res.emplace_back(root->val);
        if(root->right != nullptr) inorderTraverse(root->right, res);

        return ;
    }
    int getMinimumDifference(TreeNode* root) {
        //中序遍历递归实现
        vector<int> inorderResult;
        inorderTraverse(root, inorderResult);
        //求最小距离
        int diff = inorderResult[1] - inorderResult[0];
        int len = inorderResult.size();
        //cout << len << endl;
        for(int i=0; i<len - 1; i++){
            int temp = inorderResult[i+1] - inorderResult[i];
            diff = min(diff, temp);
        }

        return diff;
    }
};
*/
//仿照官方解答实现
class Solution {
public:
    void dfs(TreeNode* root, int& pre, int& ans){
        //边界条件
        if(root == nullptr) return ;
        //递归调用
        dfs(root->left, pre, ans);
        if(pre == -1){
            pre = root->val;
        }else{
            ans = min(ans, root->val - pre);
            pre = root->val;
        }
        dfs(root->right, pre, ans);
        return ;
    }
    int getMinimumDifference(TreeNode* root) {
        int ans = INT_MAX, pre = -1;
        dfs(root, pre, ans);
        return ans;
    }
};
```

### 知识点：

在遍历的过程中实现值的对比。

### 官方代码：

```c++
官方解答：
class Solution {
public:
    void dfs(TreeNode* root, int& pre, int& ans) {
        if (root == nullptr) {
            return;
        }
        dfs(root->left, pre, ans);
        if (pre == -1) {
            pre = root->val;
        } else {
            ans = min(ans, root->val - pre);
            pre = root->val;
        }
        dfs(root->right, pre, ans);
    }
    int getMinimumDifference(TreeNode* root) {
        int ans = INT_MAX, pre = -1;
        dfs(root, pre, ans);
        return ans;
    }
};
```

### 热评：

```
一顿操作猛如虎，耗时击败百分五
```

