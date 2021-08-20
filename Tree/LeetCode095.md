### 题目：

给你一个整数 `n` ，请你生成并返回所有由 `n` 个节点组成且节点值从 `1` 到 `n` 互不相同的不同 **二叉搜索树** 。可以按 **任意顺序** 返回答案。

### 难度：

中等。

### 示例：

```
输入：n = 3
输出：[[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]
```

### 解题思路：

```
    二叉搜索树满足的条件是：
        小于节点的值放左指针，大于节点的值放右指针。
    想法：
        1.实现起始点为loc，i个连续数字的搜索二叉树的集合
        2.遍历 1 到 n. 拼接所有可能的树。
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
    vector<TreeNode*> generateTrees(int n) {
        //返回结果
        vector<TreeNode*> res; 
        vector<TreeNode*> temp1, temp2;
        TreeNode *head = nullptr;
        //遍历1到n个数
        for(int i=1; i<=n; i++){
            //获取所有可能的左子树和右子树
            temp1 = printTrees(1, i - 1);
            temp2 = printTrees(i+1, n);
            //拼接所有左子树和右子树的组合
            for(int j=0; j<temp1.size(); j++){               
                for(int k=0; k<temp2.size(); k++){
                //创建头节点
                    head = new TreeNode{i};
                    head->left = temp1[j];
                    head->right = temp2[k];
                    res.push_back(head);
                }
            }
        }
        return res;
    }
private:
    vector<TreeNode*> printTrees(int head, int toil){
        //获取长度
        int len = toil - head + 1;
        //返回结果
        vector<TreeNode*> res;
        TreeNode *temp = nullptr;
        //边界条件
        if( len == 0){
            res.push_back(temp);
            return res;
        }
        //递归生成
        vector<TreeNode*> temp1, temp2;
        for(int i=head; i<=toil; i++){
            temp1 = printTrees(head, i - 1);
            temp2 = printTrees(i+1, toil);            
            for(int j=0; j<temp1.size(); j++){
                              
                for(int k=0; k<temp2.size(); k++){
                    temp = new TreeNode{i};
                    temp->left = temp1[j];      
                    temp->right = temp2[k];
                    res.push_back(temp);
                }
            }
        } 
        return res;
    }   
};
```

### 知识点：

  *出现的问题：*

​    *head = new TreeNode{i};*

  *必须放到循环里面。*否则会发生覆盖问题。

  *你放在外面就相当于是只存在一个根节点 root，那么你 两个循环的节点都共用这一个根节点* 

  *比如 left = [l1, l2] right = [r1, r2] 首先 root.left = l1, root.right = r1* 

  *由于你只有一个根节点，那么第二个循环的时候，还是对这个 root 进行操作。*

  *那么就会 root.left = l1, root.right = r2 的时候 就会覆盖掉原来的 root.right = r1*  

### 官方代码：

```
class Solution {
public:
    vector<TreeNode*> generateTrees(int start, int end) {
        if (start > end) {
            return { nullptr };
        }
        vector<TreeNode*> allTrees;
        // 枚举可行根节点
        for (int i = start; i <= end; i++) {
            // 获得所有可行的左子树集合
            vector<TreeNode*> leftTrees = generateTrees(start, i - 1);

            // 获得所有可行的右子树集合
            vector<TreeNode*> rightTrees = generateTrees(i + 1, end);

            // 从左子树集合中选出一棵左子树，从右子树集合中选出一棵右子树，拼接到根节点上
            for (auto& left : leftTrees) {
                for (auto& right : rightTrees) {
                    TreeNode* currTree = new TreeNode(i);
                    currTree->left = left;
                    currTree->right = right;
                    allTrees.emplace_back(currTree);
                }
            }
        }
        return allTrees;
    }

    vector<TreeNode*> generateTrees(int n) {
        if (!n) {
            return {};
        }
        return generateTrees(1, n);
    }
};
```

### 热评：

```
自己想是不可能的，这辈子都不可能自己去想的，只能靠看看评论才能做出题目。评论区里个个都是大神，思路又清晰，说话又好听，我超喜欢这里的。
```

```
每日例题，打开，看题解，关页面。 自己还是太菜了。
```

