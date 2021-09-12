### 题目：

在二叉树中，根节点位于深度 0 处，每个深度为 k 的节点的子节点位于深度 k+1 处。

如果二叉树的两个节点深度相同，但 父节点不同 ，则它们是一对堂兄弟节点。

我们给出了具有唯一值的二叉树的根节点 root ，以及树中两个不同节点的值 x 和 y 。

只有与值 x 和 y 对应的节点是堂兄弟节点时，才返回 true 。否则，返回 false。

### 难度：

简单。

### 示例：

```
输入：root = [1,2,3,4], x = 4, y = 3
输出：false
```

### 解题思路：

```c++
DFS:
    利用DFS遍历树，对每个节点进行查找
    判断其值与x, y 比较。记录节点的深度值与父节点。
    剪枝：
        如果两个数的深度都已经得到，即结束遍历
BFS:
    使用BFS，即层次遍历
    判断是否在同一层：
        如果不在，返回false。
        如果在同一层，判断父节点是否相同：
            如果相同返回false， 否则返回ture.

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
 //DFS
 /*
class Solution {
public:
    void DFS(TreeNode *root, vector<pair<int, TreeNode*>> &nodeDepth, 
        stack<TreeNode*> stk, int x, int y, int depth){
        //边界条件
        if( root == nullptr || nodeDepth.size() == 2) return ;
        //判断当前节点是否为x, y 
        if(root->val == x || root->val == y){
            if(depth == 0){
                nodeDepth.emplace_back(pair<int, TreeNode*>(0, nullptr));
            }else{
                nodeDepth.emplace_back(pair<int, TreeNode*>(depth, stk.top()));
            }
        }
        //父节点入栈
        stk.emplace(root);
        //优先调用左子树
        DFS(root->left, nodeDepth, stk, x, y, depth + 1);
        DFS(root->right, nodeDepth, stk, x, y, depth + 1);
        //父节点出栈
        stk.pop();

        return ;
    }
    bool isCousins(TreeNode* root, int x, int y) {
        //使用栈来存储节点
        stack<TreeNode*> stk;
        //存储x,y的深度和父节点
        vector<pair<int, TreeNode*>> nodeDepth;
        //深度遍历
        DFS(root, nodeDepth, stk, x, y, 0);
        //调试
        //cout << nodeDepth[0].first << "further node" << nodeDepth[0].second->val << endl;
        //cout << nodeDepth[1].first << "further node" << nodeDepth[1].second->val << endl;
        
        //判断是否为堂兄弟节点。
        int diff = nodeDepth[0].first - nodeDepth[1].first;
        bool diffFurther = (nodeDepth[0].second !=  nodeDepth[1].second);
        //cout << diffFurther ;
        if(diff == 0 && diffFurther) return true;
        return false;
    }
};
*/
//BFS
class Solution {
public:
    //pair.first:存储当前节点信息， pair.second: 存储父节点信息。
    using PTT = pair<TreeNode*, TreeNode*>;
    bool isCousins(TreeNode* root, int x, int y) {
        //使用队列实现层次遍历，来存储节点信息
        queue<PTT> q;
        q.push({root, nullptr});
        //记录查找的个数以及父节点。
        vector<TreeNode*> rec_parent;
        //层次遍历查找
        while(!q.empty()){
            //获取当前层节点的个数
            int n = q.size();
            //依次遍历当前层的每个节点
            for( int i=0; i<n; i++){
                auto [cur, parent] = q.front();
                q.pop();
                if(cur->val == x || cur->val == y) rec_parent.emplace_back(parent);
                if(cur->left) q.push({cur->left, cur});
                if(cur->right) q.push({cur->right, cur});
            }
            //判断是否得到x，y
            if( rec_parent.size() == 0 ) continue;
            if( rec_parent.size() == 1 ) return false;
            if(rec_parent.size() == 2 )  return rec_parent[0] != rec_parent[1];
        }

        return false;
    }
};
```

### 知识点：

两种做法：

​	利用深度优先遍历扫描左右子树。

​	利用广度优先遍历实现层次遍历。

### 官方代码：

```c++
官方代码：
class Solution {
private:
    // x 的信息
    int x;
    TreeNode* x_parent;
    int x_depth;
    bool x_found = false;

    // y 的信息
    int y;
    TreeNode* y_parent;
    int y_depth;
    bool y_found = false;

public:
    // 用来判断是否遍历到 x 或 y 的辅助函数
    void update(TreeNode* node, TreeNode* parent, int depth) {
        if (node->val == x) {
            tie(x_parent, x_depth, x_found) = tuple{parent, depth, true};
        }
        else if (node->val == y) {
            tie(y_parent, y_depth, y_found) = tuple{parent, depth, true};
        }
    }

    bool isCousins(TreeNode* root, int x, int y) {
        this->x = x;
        this->y = y;
        queue<pair<TreeNode*, int>> q;
        q.emplace(root, 0);
        update(root, nullptr, 0);

        while (!q.empty()) {
            auto&& [node, depth] = q.front();
            if (node->left) {
                q.emplace(node->left, depth + 1);
                update(node->left, node, depth + 1);
            }
            if (node->right) {
                q.emplace(node->right, depth + 1);
                update(node->right, node, depth + 1);
            }
            if (x_found && y_found) {
                break;
            }
            q.pop();
        }

        return x_depth == y_depth && x_parent != y_parent;
    }
};
```

### 热评：

```
直系节点和三代以内的旁系节点禁止婚配，求给定二叉树两节点是否可以结婚
```

