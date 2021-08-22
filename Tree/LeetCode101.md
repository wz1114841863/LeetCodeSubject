### 题目：

给定一个二叉树，检查它是否是镜像对称的。

### 难度：

简单。

### 示例：

```
[1,2,2,3,4,4,3]
true
```

### 解题思路：

```
错误想法：
    前序遍历和后序遍历得到的数组应该一样。
递归比较：
    先判断当前root及其左右子树是否满足条件。
    接着将左右子树看作两棵树。比较对应位置的树节点。
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
 [1,2,2,2,null,2]   
 前序遍历和后序遍历一样，但不是镜像对称的。
 */
 /*
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        //排除特殊情况
        if( root == nullptr){
            return true;
        }
        //左右数组
        vector<int> resLeft, resRight;
        //调用递归函数
        printNodeLeft(resLeft, root);
        printNodeRight(resRight, root);
        //获取长度 resLeft.size() = resRight.size()
        int len = resLeft.size();
        for(int i=0; i<len; i++){
            if(resLeft[i] != resRight[i]){
                return false;
            }
            cout << resLeft[i];
        }
        return true;
    }
    void printNodeLeft( vector<int> &res, TreeNode *root){
        //判断当前节点
        if( root == nullptr ){
            return ;
        }
        //遍历查找左节点
        if(root->left != nullptr){
            printNodeLeft(res, root->left);  
        }
        //添加当前节点
        res.push_back(root->val);
        //遍历右节点
        if(root->right != nullptr){
            printNodeLeft(res, root->right);  
        }
        //遍历结束，返回
        return ;
    }
    void printNodeRight( vector<int> &res, TreeNode *root){
        //判断当前节点
        if( root == nullptr ){
            return ;
        }
        //遍历查找左节点
        if(root->right != nullptr){
            printNodeRight(res, root->right);  
        }
        //添加当前节点
        res.push_back(root->val);
        //遍历右节点
        if(root->left != nullptr){
            printNodeRight(res, root->left);  
        }
        //遍历结束，返回
        return ;
    }
};
*/
/*
递归版本：
*/
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        //排除特殊情况
        if( root == nullptr){
            return true;
        }
        if( root->left == nullptr && root->right == nullptr){
            return true;
        }
        if( root->left == nullptr || root->right == nullptr || root->left->val != root->right->val){
            return false;
        }
        return compare(root->left, root->right);
    }
private:
    bool compare(TreeNode* node1, TreeNode* node2){
        if( node1 == nullptr &&  node2 == nullptr){
            return true;
        }
        if(node1 == nullptr ||  node2 == nullptr || node1->val !=  node2->val){
            return false;
        }
        return compare( node1->left,  node2->right) && compare( node1->right,  node2->left);
    }
};
```

### 知识点：

递归版本还没实现。

### 官方代码：

```
官方代码： 
class Solution {
public:
    bool check(TreeNode *p, TreeNode *q) {
        if (!p && !q) return true;
        if (!p || !q) return false;
        return p->val == q->val && check(p->left, q->right) && check(p->right, q->left);
    }

    bool isSymmetric(TreeNode* root) {
        return check(root, root);
    }
};
```

### 热评：

```
可以自己从2ms改到1ms、0ms，这应该就是这几个月来的明显收获了吧！
```

