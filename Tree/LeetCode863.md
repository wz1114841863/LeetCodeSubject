### 题目：

给定一个二叉树（具有根结点 root）， 一个目标结点 target ，和一个整数值 K 。

返回到目标结点 target 距离为 K 的所有结点的值的列表。 答案可以以任何顺序返回。

### 难度：

简单。

### 示例：

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, K = 2
输出：[7,4,1]
解释：
所求结点为与目标结点（值为 5）距离为 2 的结点，
值分别为 7，4，以及 1
```

### 解题思路：

```c++
初始想法：
    利用深度优先遍历(DFS)。记录节点的所有父节点。
    查找到目标节点target后，距离为K的节点可能在：
        1.以target为根节点的第K层所有的子节点。这里要用BFS来实现。
        2.其父节点的其它子树的子节点。
            获取到父节点中的另一子树。（不在当前路径内的）
            设其父节点距离其距离为m,求其除当前路径外的其他子树中第（k -m)层的子节点。
更多了解到的想法：
    1.从二叉树建图，然后使用遍历(DFS/BFS)搜索
    2.利用哈希表，从traget开始，实现向左子树，右子树，父节点三个方向搜索。
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
    //利用dfs获取父亲节点
    bool dfs(TreeNode* root, vector<TreeNode*> &fatherNodes, TreeNode* target){
        //边界条件
        if(root == nullptr ) return false;
        if(root == target){
            fatherNodes.emplace_back(root);
            return true;
        }
        //存储当前节点
        fatherNodes.emplace_back(root);
        if( dfs(root->left, fatherNodes, target) ) return true;
        if( dfs(root->right, fatherNodes, target) ) return true;
        fatherNodes.pop_back();

        return false;
    }
    //参数：根节点， 返回值， 距离也就是深度。
    void bfs(TreeNode* root, vector<int>& res, int distance){
        //排除特殊情况，输入的节点就是空节点。
        if(root == nullptr) return ;
        //层次遍历，可能没有满足条件的结果
        queue<TreeNode*> q;
        q.push(root);
        //记录层数
        //root为第一层。
        int level = 1;
        while(!q.empty()){
            //获取当前队列长度
            int currentLevelSize = q.size();
            //比较层数和所要查找的深度
            if(level == distance){
                for(int i=0; i<currentLevelSize; i++){

                    TreeNode* node = q.front();
                    q.pop();
                    res.emplace_back(node->val);
                }  

                return ;              
            }
            //深度不到，继续进行层次遍历
            for(int i=0; i<currentLevelSize; i++){
                TreeNode* node = q.front();
                q.pop();
                if(node->left != nullptr) q.push(node->left);
                if(node->right != nullptr) q.push(node->right);
            }   
            ++level;         
        }
    }
    vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
        //首先查找target节点以及记录其路径（父节点），包括自己本身。
        vector<TreeNode*> fatherNodes{};
        dfs(root, fatherNodes, target);
        //调试
        //cout << fatherNodes.size() << endl;
        //返回结果res
        vector<int> res{};
        //先查找以target为根节点的第 k+1 层子节点。
        //根节点为第一层，距离为k，所以 distance = k + 1.
        bfs(target, res, k + 1);
        //调试
        //cout << res.size() << endl;
        //对父亲节点进行遍历
        int len = fatherNodes.size();
        int distance = 0;
        //不用遍历当前节点，所以从 len-2开始遍历。
        //如果 len > k, 会从 distance == k 结束遍历。
        for(int i=len-2; i >= 0; i--){
            //调试
            //cout << fatherNodes[i]->val << endl;
            //distance = 1;
            //计算距离
            distance++;
            //获取父节点
            TreeNode* node = fatherNodes[i];
            //判断是否即为当前树节点。
            if(distance == k){
                res.emplace_back(node->val);
                break;
            }
            //查找其那个子树存储在fatherNodes中，进而对另一个子树进行查找
            if(node->left == fatherNodes[i+1]){
                //左子树在路径中
                //cout << "调用 right：" << endl;
                bfs(node->right, res, k - distance);
            }else{
                //右子树在路径中
                //cout << "调用 left：" << endl;
                bfs(node->left, res, k - distance);
            }
        }

        return res;
    }
};
```

### 知识点：

如何从二叉树建立图。

### 官方代码：

```c++
class Solution {
    unordered_map<int, TreeNode*> parents;
    vector<int> ans;

    void findParents(TreeNode* node) {
        if (node->left != nullptr) {
            parents[node->left->val] = node;
            findParents(node->left);
        }
        if (node->right != nullptr) {
            parents[node->right->val] = node;
            findParents(node->right);
        }
    }

    void findAns(TreeNode* node, TreeNode* from, int depth, int k) {
        if (node == nullptr) {
            return;
        }
        if (depth == k) {
            ans.push_back(node->val);
            return;
        }
        if (node->left != from) {
            findAns(node->left, node, depth + 1, k);
        }
        if (node->right != from) {
            findAns(node->right, node, depth + 1, k);
        }
        if (parents[node->val] != from) {
            findAns(parents[node->val], node, depth + 1, k);
        }
    }

public:
    vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
        // 从 root 出发 DFS，记录每个结点的父结点
        findParents(root);

        // 从 target 出发 DFS，寻找所有深度为 k 的结点
        findAns(target, nullptr, 0, k);

        return ans;
    }
};
```

### 热评：

```
停电我依然做题。卷出一片天。
```

