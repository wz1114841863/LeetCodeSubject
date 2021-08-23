### 题目：

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明:** 叶子节点是指没有子节点的节点。

### 难度：

简单。

### 示例：

```
给定二叉树 [3,9,20,null,null,15,7]，

    3
   / \
  9  20
    /  \
   15   7
返回它的最大深度 3 。
```

### 解题思路：

```
    初始想法：
        其实前两题已经求出了树的深度。非递归。
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
    int maxDepth(TreeNode* root) {
        //排除特殊情况
        if(root == nullptr){
            return 0;
        }
        //返回结果
        int depth;
        //存储指针和向量
        vector<TreeNode *> node1, node2;
        //node1存储上一层的所有指针
        node1.push_back(root);
        depth++;
        while(!node1.empty()){
            node2.clear();
            int len = node1.size();
            for(int i=0; i<len; i++){
                if(node1[i]->left != nullptr){
                    node2.push_back(node1[i]->left);
                }
                if(node1[i]->right != nullptr){
                    node2.push_back(node1[i]->right);
                }
            }
            node1 = node2;
            depth++;
        }

        return depth;
    }
};
```

### 知识点：

属于BFS的写法，此外还有DFS，递归等其他写法。

### 官方代码：

```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == nullptr) return 0;
        queue<TreeNode*> Q;
        Q.push(root);
        int ans = 0;
        while (!Q.empty()) {
            int sz = Q.size();
            while (sz > 0) {
                TreeNode* node = Q.front();Q.pop();
                if (node->left) Q.push(node->left);
                if (node->right) Q.push(node->right);
                sz -= 1;
            }
            ans += 1;
        } 
        return ans;
    }
};
```

### 热评：

```
//打败100%
class Solution {
    public int maxDepth(TreeNode root) {
        return root == null ? 0 : Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```

```
我发誓这次我没有复制
```

