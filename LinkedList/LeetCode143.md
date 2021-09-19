### 题目：

给定一个单链表 L 的头节点 head ，单链表 L 表示为：

 L0 → L1 → … → Ln-1 → Ln 
请将其重新排列后变为：

L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → …

不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

### 难度：

中等。

### 示例：

```
输入: head = [1,2,3,4]
输出: [1,4,2,3]
```

### 解题思路：

```c++
将以前做的题结合起来：
    1.利用快慢指针找到中点，分为前后两部分。
    2.将后一部分逆序排列。
    3.将两个链表重新合并后排列。
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
    void reorderList(ListNode* head) {
        //排除特殊情况,避免后一部分链表为空
        if(head == nullptr || head->next == nullptr) return ;
        //使用快慢指针找到中点，将链表分为前后分割为两个链表
        ListNode *fast = head->next, *slow = head;
        while(fast && fast->next){
            fast = fast->next;
            fast = fast->next;
            slow = slow->next;
        }
        //slow此时指向中点的前一个。
        ListNode* sec = new ListNode(-1, slow->next);
        slow->next = nullptr;
        //将sec链表逆序
        //使用头插法
        ListNode *cur = sec->next ,*node = nullptr;
        while(cur->next){
            node = cur->next;
            cur->next = node->next;
            node->next = sec->next;
            sec->next = node;
        }
        //将后半部分插入到前版部分
        cur = head;
        node = sec->next;
        while(node){
            sec->next = node->next;
            node->next = cur->next;
            cur->next = node;
            cur = node->next;
            node = sec->next;
        }

        return ;
    }
};
```

### 知识点：

最优解法

除此之外还可以利用向量存储和递归。

### 官方代码：

```c++
class Solution {
public:
    void reorderList(ListNode* head) {
        if (head == nullptr) {
            return;
        }
        ListNode* mid = middleNode(head);
        ListNode* l1 = head;
        ListNode* l2 = mid->next;
        mid->next = nullptr;
        l2 = reverseList(l2);
        mergeList(l1, l2);
    }

    ListNode* middleNode(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast->next != nullptr && fast->next->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }

    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        while (curr != nullptr) {
            ListNode* nextTemp = curr->next;
            curr->next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }

    void mergeList(ListNode* l1, ListNode* l2) {
        ListNode* l1_tmp;
        ListNode* l2_tmp;
        while (l1 != nullptr && l2 != nullptr) {
            l1_tmp = l1->next;
            l2_tmp = l2->next;

            l1->next = l2;
            l1 = l1_tmp;

            l2->next = l1;
            l2 = l2_tmp;
        }
    }
};
```

### 热评：

```
。今天的字节面试题, 最优解通过。
```

