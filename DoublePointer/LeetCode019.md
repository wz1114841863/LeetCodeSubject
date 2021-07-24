### 题目：

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

**进阶：**你能尝试使用一趟扫描实现吗？

### 难度：

中等。

### 示例：

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

### 解题思路：

```
初始想法：
利用两个指针，first指针先走n步，second指针指向first - n 位置。
当first 到底时，处理second。
最后返回head。
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
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        //首先排除一些特殊情况
        if((!head) || (!head->next)){
            return nullptr;
        }
        //初始化两个指针first和second。
        ListNode *first = head;
        ListNode *second = head;
        //计数
        int count = 0;
        //开始遍历list
        while(first->next != nullptr){
            first = first->next;
            count++;
            if(count > n){
                second = second->next;
            }
        }
        //如果要删除的就是第一个节点。
        if(( second == head) && (count != n)){
            return head->next;
        }
        //删除节点
        ListNode *index = second->next;
        second->next = index->next;
        delete index;
        return head;
    }
};
```

### 知识点：

快慢指针：

快指针遍历内容，作为边界条件截止，慢指针作为答案的位置（不管是插入还是查询），作为快指针停止后停留在答案对应的位置。

### 官方代码：

```
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0, head);
        ListNode* first = head;
        ListNode* second = dummy;
        for (int i = 0; i < n; ++i) {
            first = first->next;
        }
        while (first) {
            first = first->next;
            second = second->next;
        }
        second->next = second->next->next;
        ListNode* ans = dummy->next;
        delete dummy;
        return ans;
    }
};

```

### 热评：

```
今天这题快慢指针，简单。
```

