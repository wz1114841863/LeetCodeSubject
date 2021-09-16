### 题目：

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

### 难度：

简单。

### 示例：

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

### 解题思路：

```c++
记录指向前一节点的指针。
暂存指向下一节点的指针。
修改当前指针的next指向。
遍历完毕后返回。
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
    ListNode* reverseList(ListNode* head) {
        //排除特殊情况
        if(!head || !head->next) return head;
        //分别存储上一节点，当前节点，和下一节点的指针
        ListNode *pre = head, *cur = head->next, *temp=cur->next;
        cur->next = head;
        head->next = nullptr;
        head = cur;
        //还有节点未处理
        while(temp){
            cur = temp;
            temp = cur->next;
            cur->next = head;
            head = cur;
        }
        return cur;
    }
};
```

### 知识点：

还可以利用迭代来实现。

### 官方代码：

```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !head->next) {
            return head;
        }
        ListNode* newHead = reverseList(head->next);
        head->next->next = head;
        head->next = nullptr;
        return newHead;
    }
};
```

### 热评：

```
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode ans = null;
        for (ListNode x = head; x != null; x = x.next) {
            ans = new ListNode(x.val,ans);
        }
        return ans;
    }
}
```

