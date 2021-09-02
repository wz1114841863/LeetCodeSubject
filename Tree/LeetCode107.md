### 题目：

给定一个二叉树，返回其节点值自底向上的层序遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）



### 难度：



### 示例：

```
给定二叉树： 
[3,9,20,null,null,15,7]
返回其自底向上的层序遍历为：
[
  [15,7],
  [9,20],
  [3]
]
```

### 解题思路：

```
与层次遍历相同，只不过插入时将向量，插入到的前面。
与其插入到最前面，不如直接翻转。
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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        //返回结果
        vector<vector<int>> res;
        //排除特殊情况
        if(root == nullptr){
            return res;
        }
        //存储指针和向量
        vector<TreeNode *> node1, node2;
        vector<int> temp;
        //node1存储上一层的所有指针
        node1.push_back(root);
        while(!node1.empty()){
            temp.clear();
            node2.clear();
            int len = node1.size();
            for(int i=0; i<len; i++){
                temp.push_back(node1[i]->val);
                if(node1[i]->left != nullptr){
                    node2.push_back(node1[i]->left);
                }
                if(node1[i]->right != nullptr){
                    node2.push_back(node1[i]->right);
                }
            }
            node1 = node2;
            //如果每次都插入到最前面的话，相当于每次都要移动后面所有的元素。
            //不如全部完成后翻转。
            //res.insert(res.begin() ,temp);
            res.push_back(temp);
        }
        //原地翻转
        reverse(res.begin(), res.end());
        return res;
    }
};
```

### 知识点：

应该有更好的解决方案，避免翻转操作。deque或许更好一点。

### 官方代码：

```c++
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> levelOrder = new LinkedList<List<Integer>>();
        if (root == null) {
            return levelOrder;
        }
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            List<Integer> level = new ArrayList<Integer>();
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                level.add(node.val);
                TreeNode left = node.left, right = node.right;
                if (left != null) {
                    queue.offer(left);
                }
                if (right != null) {
                    queue.offer(right);
                }
            }
            levelOrder.add(0, level);
        }
        return levelOrder;
    }
}
```

### 热评：

```
1.把之前做过的 102 题的层序遍历拿出来
2.在 return 之前加一句 reverse(ans.begin(), ans.end());
3.提交，通过
美好的一天从现在开始
```

