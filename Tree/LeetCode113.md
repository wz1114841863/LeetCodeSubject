### 题目：

给你二叉树的根节点 root 和一个整数目标和 targetSum ，找出所有 从根节点到叶子节点 路径总和等于给定目标和的路径。

叶子节点 是指没有子节点的节点。

### 难度：

中等。

### 示例：

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：[[5,4,11,2],[5,8,4,5]]
```

### 解题思路：

```c++
仿照上一题的代码，只不过需要遍历所有节点。
依旧使用递归。
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
    void findPath(const int targetSum, vector<vector<int>> &res, vector<int> &temp, 
            TreeNode *root, int sum){
        //边界条件
        if( root == nullptr ){
            return ;
        }
        if( root->left == nullptr && root->right == nullptr ){
            //是满足条件的根节点
            if(targetSum - sum == root->val){
                //添加到res
                temp.push_back(root->val);
                res.push_back(temp);
                temp.pop_back();
            }

            return ;
        }
        //将当前节点添加到temp中
        temp.push_back(root->val);
        //递归调用
        //左子树
        findPath(targetSum, res, temp, root->left, sum + root->val);
        //右子树
        findPath(targetSum, res, temp, root->right, sum + root->val);
        //弹出当前节点的值
        temp.pop_back();
        

        //当前节点结束调用
        return ;
    }
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
       //返回结果
        vector<vector<int>> res;
        //暂存值
        vector<int> temp;
        //累计和 
        int sum = 0;
        //调用递归函数
        findPath(targetSum, res, temp, root, sum);
        return res;
    }
};
```

### 知识点：

使用递归访问所有可能的节点。

### 官方代码：

```c++
class Solution {
public:
    vector<vector<int>> ret;
    unordered_map<TreeNode*, TreeNode*> parent;

    void getPath(TreeNode* node) {
        vector<int> tmp;
        while (node != nullptr) {
            tmp.emplace_back(node->val);
            node = parent[node];
        }
        reverse(tmp.begin(), tmp.end());
        ret.emplace_back(tmp);
    }

    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        if (root == nullptr) {
            return ret;
        }

        queue<TreeNode*> que_node;
        queue<int> que_sum;
        que_node.emplace(root);
        que_sum.emplace(0);

        while (!que_node.empty()) {
            TreeNode* node = que_node.front();
            que_node.pop();
            int rec = que_sum.front() + node->val;
            que_sum.pop();

            if (node->left == nullptr && node->right == nullptr) {
                if (rec == targetSum) {
                    getPath(node);
                }
            } else {
                if (node->left != nullptr) {
                    parent[node->left] = node;
                    que_node.emplace(node->left);
                    que_sum.emplace(rec);
                }
                if (node->right != nullptr) {
                    parent[node->right] = node;
                    que_node.emplace(node->right);
                    que_sum.emplace(rec);
                }
            }
        }

        return ret;
    }
};
```

### 热评：

```
亥 我还加了个大于sum就剪枝的流程，然后解答错误..
```

