### 题目：

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。

### 难度：

中等。

### 示例：

```
输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。
```

### 解题思路：

```
判断中序遍历后是否为有序向量。
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
    bool isValidBST(TreeNode* root) {
        //非递归中序遍历
         //返回结果
        vector<int> res;
        //处理特殊情况
        if( root == nullptr){
            return true;
        } 
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
            if( temp == nullptr || flag == 2){
                //空指针,弹出
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
                //添加节点信息
                res.push_back(temp->val);
                //修改s2
                s2.pop();
                s2.push(2);
                //存储右节点
                s1.push(temp->right);
                s2.push(0);
            }
        }
        //判断是否有序
        int len = res.size();
        /*
        for( int i=0; i<len; i++){
            cout << res[i] << endl;
        }
        */
        for(int i = 1; i < len; i++){
            
            if( res[i] <= res[i-1]){
                return false;
            }            
        }
        return true;
    }   
};
/*
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> stack;
        long long inorder = (long long)INT_MIN - 1;

        while (!stack.empty() || root != nullptr) {
            while (root != nullptr) {
                stack.push(root);
                root = root -> left;
            }
            root = stack.top();
            stack.pop();
            // 如果中序遍历得到的节点的值小于等于前一个 inorder，说明不是二叉搜索树
            if (root -> val <= inorder) {
                return false;
            }
            inorder = root -> val;
            root = root -> right;
        }
        return true;
    }
};
```

### 知识点：

中序遍历还可以使用迭代版本，也可以在中序遍历过程中比较大小。

除此之外整个题目还可以通过递归来做。

### 官方代码：

```
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> stack;
        long long inorder = (long long)INT_MIN - 1;

        while (!stack.empty() || root != nullptr) {
            while (root != nullptr) {
                stack.push(root);
                root = root -> left;
            }
            root = stack.top();
            stack.pop();
            // 如果中序遍历得到的节点的值小于等于前一个 inorder，说明不是二叉搜索树
            if (root -> val <= inorder) {
                return false;
            }
            inorder = root -> val;
            root = root -> right;
        }
        return true;
    }
};
```

### 热评：

```
中序遍历为升序。
```

