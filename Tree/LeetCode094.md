### 题目：

给定一个二叉树的根节点 `root` ，返回它的 **中序** 遍历。

### 难度：

简单。

### 示例：

```
输入：root = [1,null,2,3]
输出：[1,3,2]
```

### 解题思路：

```
    经典: 二叉树的中序遍历。
递归：
    先查找左节点，查找到最深处，打印。
    然后打印中间节点。
    然后查找从右节点开始从新查找。
迭代：
    利用栈来存储遍历过的，每一节点。
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
    vector<int> inorderTraversal(TreeNode* root) {
        //返回结果
        vector<int> res;
        printNode(res, root);  
        return res;
    }
private:
    void printNode( vector<int> &res, TreeNode *root){
        //判断当前节点
        if( root == nullptr ){
            return ;
        }
        //遍历查找左节点
        if(root->left != nullptr){
            printNode(res, root->left);  
        }
        //添加当前节点
        res.push_back(root->val);
        //遍历右节点
        if(root->right != nullptr){
            printNode(res, root->right);  
        }
        //遍历结束，返回
        return ;
    }
};
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        //返回结果
        vector<int> res;
        //处理特殊情况
        if( root == nullptr){
            return res;
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
        return res;
    }

};
```

### 知识点：

三种遍历方式都要熟悉。

### 官方代码：

```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        TreeNode *predecessor = nullptr;

        while (root != nullptr) {
            if (root->left != nullptr) {
                // predecessor 节点就是当前 root 节点向左走一步，然后一直向右走至无法走为止
                predecessor = root->left;
                while (predecessor->right != nullptr && predecessor->right != root) {
                    predecessor = predecessor->right;
                }
                
                // 让 predecessor 的右指针指向 root，继续遍历左子树
                if (predecessor->right == nullptr) {
                    predecessor->right = root;
                    root = root->left;
                }
                // 说明左子树已经访问完了，我们需要断开链接
                else {
                    res.push_back(root->val);
                    predecessor->right = nullptr;
                    root = root->right;
                }
            }
            // 如果没有左孩子，则直接访问右孩子
            else {
                res.push_back(root->val);
                root = root->right;
            }
        }
        return res;
    }
};
```

### 热评：

```
递归真香
```

