### 题目：

给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。

### 难度：

简单。

### 示例：

```
输入：head = [1,2,2,1]
输出：true
```

### 解题思路：

```c++

	1.使用快慢指针将链表分为两个部分
        如果链表节点个数为偶数，则两部分长度相同。
        如果链表节点个数为奇数，则两部分前一部分长度比后一部分多一。
    2.将后一部分链表逆序，然后与前一部分链表逐次比较。
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
    bool isPalindrome(ListNode* head) {
        //排除特殊情况
        if(head == nullptr || head->next == nullptr) return true;
        //使用快慢指针将链表分为两部分
        ListNode* fast = head->next, *slow = head;
        while(fast && fast->next){
            fast = fast->next->next;
            slow = slow->next;
        }
        ListNode *sec = slow->next;
        slow->next = nullptr;
        //使用头插法将后一部分链表翻转
        ListNode *dummyNode = new ListNode(-1, sec), *node = nullptr;
        while(sec->next){
            node = sec->next;
            sec->next = node->next;
            node->next = dummyNode->next;
            dummyNode->next = node;
        }
        sec = dummyNode->next;
        //比较
        while(head && sec){
            if(head->val != sec->val){
                return false;
            }
            head = head->next;
            sec = sec->next;
        }

        return true;
    }
};
```

### 知识点：

问题：

​	破环了原有链表的结构。

​	可以再将后半部分链表进行逆序操作后，重新拼接。

​	在并发环境下，函数运行时需要锁定其他线程或进程对链表的访问，因为在函数执行过程中链表会被修改。

### 官方代码：

```c++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if (head == nullptr) {
            return true;
        }

        // 找到前半部分链表的尾节点并反转后半部分链表
        ListNode* firstHalfEnd = endOfFirstHalf(head);
        ListNode* secondHalfStart = reverseList(firstHalfEnd->next);

        // 判断是否回文
        ListNode* p1 = head;
        ListNode* p2 = secondHalfStart;
        bool result = true;
        while (result && p2 != nullptr) {
            if (p1->val != p2->val) {
                result = false;
            }
            p1 = p1->next;
            p2 = p2->next;
        }        

        // 还原链表并返回结果
        firstHalfEnd->next = reverseList(secondHalfStart);
        return result;
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

    ListNode* endOfFirstHalf(ListNode* head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while (fast->next != nullptr && fast->next->next != nullptr) {
            fast = fast->next->next;
            slow = slow->next;
        }
        return slow;
    }
};
```

### 热评：

```
我累了。
```

