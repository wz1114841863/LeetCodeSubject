### 题目：

给定一个二叉树，返回它的 *后序* 遍历。

### 难度：

简单。

### 示例：

```
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [3,2,1]
```

### 解题思路：

```
改一下递归调用顺序。
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
 //递归
 /*
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        //返回结果
        vector<int> res;
        preorder(root, res);
        return res;       
    }
private:
    void preorder(TreeNode *root, vector<int> &res){
        //判断非空指针
        if(root == nullptr){
            return ;
        }
        
        if(root->left != nullptr)
            preorder(root->left, res);
        if(root->right != nullptr)
            preorder(root->right, res);
        res.push_back(root->val);
        return ;
    }
};
*/
//遍历
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        //返回结果
        vector<int> res;
        //利用栈存储
        stack<TreeNode*> s1;
        stack<int> s2;
        TreeNode *temp = nullptr;
        //flag == 0, 1, 2
        //0:第一次，1：第二次。 2：第三次
        int flag = 0;
        s1.push(root);
        s2.push(0);
        while(! s1.empty()){
            temp = s1.top();
            flag = s2.top();
            //cout << "temp:  " << temp <<"  flag:  " << flag << endl;
            if( temp == nullptr){
                //空指针,弹出
                s1.pop();
                s2.pop();
                continue;
            }
            if(flag == 2){
                //添加节点信息
                res.push_back(temp->val);
                //已经遍历并且存储值
                s1.pop();
                s2.pop();
                continue;
            }
            //第一次遍历
            if(flag == 0){
                //修改当前节点的flag
                s2.pop();
                s2.push(1);
                //存储左节点
                s1.push(temp->left);
                s2.push(0);
            }else{

                //修改s2
                s2.pop();
                s2.push(2);
                //存储右节点
                s1.push(temp->right);
                s2.push(0);
            }
        }
        return res;
    }
};
```

### 知识点：

先序遍历： 根节点 -> 左节点 -> 右节点

中序遍历： 左节点->根节点 -> 右节点

后序遍历： 左节点-> 右节点-> 根节点

### 官方代码：

```
class Solution {
public:
    vector<int> postorderTraversal(TreeNode *root) {
        vector<int> res;
        if (root == nullptr) {
            return res;
        }

        stack<TreeNode *> stk;
        TreeNode *prev = nullptr;
        while (root != nullptr || !stk.empty()) {
            while (root != nullptr) {
                stk.emplace(root);
                root = root->left;
            }
            root = stk.top();
            stk.pop();
            if (root->right == nullptr || root->right == prev) {
                res.emplace_back(root->val);
                prev = root;
                root = nullptr;
            } else {
                stk.emplace(root);
                root = root->right;
            }
        }
        return res;
    }
};

```

### 热评：

```
为什么北邮有这么多的神仙啊。
```

