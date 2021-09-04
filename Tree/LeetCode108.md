### 题目：

给你一个整数数组 nums ，其中元素已经按 升序 排列，请你将其转换为一棵 高度平衡 二叉搜索树。

高度平衡 二叉树是一棵满足「每个节点的左右两个子树的高度差的绝对值不超过 1 」的二叉树。

### 难度：

简单。

### 示例：

```
输入：nums = [-10,-3,0,5,9]
输出：[0,-3,9,-10,null,5]
解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案：
```

### 解题思路：

```c++
    搜索树满足：
        大的数在其右子树，小的数在其左子树。
    平衡树满足：
        每个节点的左右两个子树的高度差的绝对值不超过 1 。
    从nums中间开始创建，中间的树作为根节点，如果为奇数，根节点唯一，如果为偶数，取中点右边的数字为根节点。
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
    TreeNode* sortedArrayToBST(vector<int>& nums) {
            //获取长度 [0, len)
            int len = nums.size();
            return buildChildTree(nums, 0, len); 
    }
private:
    //左开右闭区间： [start, end)
    TreeNode* buildChildTree(vector<int>& nums, int start, int end){
        //获取中间值
        int middle = ( end + start ) / 2;
        //边界情况
        if(start == (end - 1)){
            return new TreeNode(nums[middle]);
        }else if(start == end){
            return nullptr;
        }
        TreeNode *root = new TreeNode(nums[middle]);
        root->left = buildChildTree(nums, start, middle);
        root->right = buildChildTree(nums, middle + 1, end); 
        return root;      
    }
};
```

### 知识点：

```
求中点最好用：
	int middle = left + (right - left)/ 2;
而不是：
	int middle = (left + right ) / 2;
防止溢出。如果left 和 right 都很大的话，第二种方式很可能就会溢出。
```

### 官方代码：

```c++
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return helper(nums, 0, nums.size() - 1);
    }

    TreeNode* helper(vector<int>& nums, int left, int right) {
        if (left > right) {
            return nullptr;
        }

        // 总是选择中间位置右边的数字作为根节点
        int mid = (left + right + 1) / 2;

        TreeNode* root = new TreeNode(nums[mid]);
        root->left = helper(nums, left, mid - 1);
        root->right = helper(nums, mid + 1, right);
        return root;
    }
};
```

### 热评：

```
果然刷题的动力来源于自己写代码，并且一次就ac时的快感。对于菜鸡的我来说，即使是简单级别一次性通过，也能获得巨大的成就感。革命尚未成功，还需继续努力。
```

