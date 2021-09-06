### 题目：

给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

### 难度：

中等。

### 示例：

```
给定的有序链表： [-10, -3, 0, 5, 9],

一个可能的答案是：[0, -3, 9, -10, null, 5], 它可以表示下面这个高度平衡二叉搜索树：

      0
     / \
   -3   9
   /   /
 -10  5
```

### 解题思路：

```c++
初始想法：
    可以将遍历链表，将所有的数存到一个向量中nums中，就成了上一题。
    这样虽然可以实现，但是会浪费专门用来遍历链表的时间。
    时间复杂度为O(2*n).第一个n是遍历整个链表，第二个n是遍历整个向量。
    空间复杂度为O(n + logn),n是向量nums 的大小， logn 是递归的深度。
改进想法：
    利用快慢指针来查找中间节点的位置，同时利用递归建立左右子树。
```

### 代码实现：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
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
    初始想法实现
 */
 /*
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        //建立一个向量nums
        vector<int> nums;
        while(head != nullptr){
            nums.push_back(head->val);
            head = head->next;
        }
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
*/
//改进想法： 阅读了官方题解之后。
//左闭右开
class Solution {
public:
    //快慢指针。
    ListNode* getMiddle(ListNode* left, ListNode* right){
        ListNode *fast = left, *slow = left;
        while(fast != right && fast->next != right){
            fast = fast->next;
            fast = fast_.next;
            slow = slow->next;
        }
        
        return slow;
    }
    TreeNode* buildChildTree(ListNode* left, ListNode* right){
        //边界条件
        if(left == right){
            return nullptr;
        }
        //生成根节点
        ListNode* middle = getMiddle(left, right);
        TreeNode* root = new TreeNode(middle->val);
        root->left = buildChildTree(left, middle);
        root->right = buildChildTree(middle->next, right);
    }
    TreeNode* sortedListToBST(ListNode* head) {
        return buildChildTree(head, nullptr);
    }
};
```

### 知识点：

使用快慢指针来确定中间节点位置。

### 官方代码：

```c++
官方解法：
class Solution {
public:
    int getLength(ListNode* head) {
        int ret = 0;
        for (; head != nullptr; ++ret, head = head->next);
        return ret;
    }

    TreeNode* buildTree(ListNode*& head, int left, int right) {
        if (left > right) {
            return nullptr;
        }
        int mid = (left + right + 1) / 2;
        TreeNode* root = new TreeNode();
        root->left = buildTree(head, left, mid - 1);
        root->val = head->val;
        head = head->next;
        root->right = buildTree(head, mid + 1, right);
        return root;
    }

    TreeNode* sortedListToBST(ListNode* head) {
        int length = getLength(head);
        return buildTree(head, 0, length - 1);
    }
};
```

### 热评：

```
链表如何一遍扫描找中点, 新技能Get
```

