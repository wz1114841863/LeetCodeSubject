### 题目：

给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

**说明：**叶子节点是指没有子节点的节点。

### 难度：

简单。

### 示例：

```
输入：root = [3,9,20,null,null,15,7]
输出：2
```

### 解题思路：

```
层次遍历，返回最先找到的叶子节点的深度。
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
    int minDepth(TreeNode* root) {
        //返回结果
        int depth = 0;
        //排除特殊情况
        if(root == nullptr) return depth;
        //临时变量
        vector<TreeNode *> temp1, temp2;
        temp1.push_back(root);
        depth++;
        TreeNode *node;
        int len = 0;
        while(1){
            len = temp1.size();
            //遍历查看每一层的每一个数据是不是树节点。
            for(int i=0; i<len; i++){
                node = temp1[i];
                //根节点
                if( node->left == nullptr && node->right == nullptr){
                    return depth;
                }
                if(node->left != nullptr){
                    temp2.push_back(node->left);
                }
                if(node->right != nullptr){
                    temp2.push_back(node->right);
                }
            }
            //如果没有返回，深度加一
            depth++;
            temp1 = temp2;
            temp2.clear();
        }

        return depth;
    }
};
```

### 知识点：

也可以用遍历来解决。树的很多问题都可以用遍历来解决。

### 官方代码：

```
class Solution {
public:
    int minDepth(TreeNode *root) {
        if (root == nullptr) {
            return 0;
        }

        if (root->left == nullptr && root->right == nullptr) {
            return 1;
        }

        int min_depth = INT_MAX;
        if (root->left != nullptr) {
            min_depth = min(minDepth(root->left), min_depth);
        }
        if (root->right != nullptr) {
            min_depth = min(minDepth(root->right), min_depth);
        }

        return min_depth + 1;
    }
};
```

### 热评：

```
不愧是简单题 就是简单.
```

