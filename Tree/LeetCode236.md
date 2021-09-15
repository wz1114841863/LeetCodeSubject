### 题目：

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。

### 难度：

中等。

### 示例：

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
```

### 解题思路：

```c++
利用DFS查找其中任意一个节点，并存储其路径。
    1.查找firstNode的左右子树
    2.根据查找到的路径查找其不在节点中的另一子树。
时间复杂度取决于节点的远近程度。最坏情况下会遍历所有节点。O(N)
空间复杂度:
    1.DFS递归调用
    2.利用stack存储父节点。
其他想法：
1.记录两个节点的路径，进行比较
    1.利用哈希表分别记录两个节点的父节点路径
    2.首先查找p点的所有父节点，并记录
    3.从下向上查找q的所有父节点一旦发现已经有记录过的父节点，即返回。

```

### 代码实现：

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode *p = nullptr, *q = nullptr;
    //记录节点查找到的顺序：
    TreeNode *firstNode, *secondNode;
    bool dfsFirstNode(TreeNode* root, stack<TreeNode*> &path){
        //返回条件
        if(root == nullptr) return false;
        if(root == p){
            this->firstNode = p;
            this->secondNode = q;
            return true;
        }else if( root == q){
            this->firstNode = q;
            this->secondNode = p;
            return true;
        }
        //节点入栈
        path.push(root);
        //递归查询左右子树
        if( dfsFirstNode(root->left, path) ) return true;
        if( dfsFirstNode(root->right, path) ) return true;
        //出栈
        path.pop();

        return false;

    }
    bool dfsSecondNode(TreeNode* root){
        //边界条件
        if(root == nullptr) return false;
        if(root == this->secondNode) return true;
        if(dfsSecondNode(root->left)) return true;
        if(dfsSecondNode(root->right)) return true;
        return false;
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        //赋值
        this->p = p;
        this->q = q;
        //记录firstNode的路径
        stack<TreeNode*> path{};
        //查找第一个节点路径
        dfsFirstNode(root, path);
        //查找第二个节点路径
        //对栈中的节点进行查找
        if(dfsSecondNode(this->firstNode)) return firstNode;
        //查找其父节点路径
        TreeNode *preNode = this->firstNode;
        //调试
        //cout << "first->val " << (this->firstNode)->val << endl;
        while(!path.empty()){
            TreeNode* node = path.top();
            //调试
            //cout << "preNode->val " << preNode->val << endl;
            //cout << "node->val " << node->val << endl;
            path.pop();
            bool flag = false;
            if(node->left == preNode){
                flag = dfsSecondNode(node->right);
            }else{
                flag = dfsSecondNode(node->left);
            }
            if(flag) return node;
            preNode = node;
        } 

        return nullptr;
    }
};
```

### 知识点：

做题之前，考虑是要用什么样的遍历方式。

### 官方代码：

```c++
class Solution {
public:
    unordered_map<int, TreeNode*> fa;
    unordered_map<int, bool> vis;
    void dfs(TreeNode* root){
        if (root->left != nullptr) {
            fa[root->left->val] = root;
            dfs(root->left);
        }
        if (root->right != nullptr) {
            fa[root->right->val] = root;
            dfs(root->right);
        }
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        fa[root->val] = nullptr;
        dfs(root);
        while (p != nullptr) {
            vis[p->val] = true;
            p = fa[p->val];
        }
        while (q != nullptr) {
            if (vis[q->val]) return q;
            q = fa[q->val];
        }
        return nullptr;
    }
};

```

### 热评：

```
程序员两大疑问： 1.这段代码为什么会有问题？ 2.这段代码为什么会没有问题？
```

