### 题目：

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

**你不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换。

### 难度：

中等。

### 示例：

```
输入：head = [1,2,3,4]
输出：[2,1,4,3]
```

### 解题思路：

```
1.排除特殊情况
2.n >= 2; 交换头两个节点。然后递归剩下的 n-2 个节点、
需要注意的是，需要传递给函数他的父节点，保证链表不会断。
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
    ListNode* swapPairs(ListNode* head) {
        //排除空链表和只有一个节点的情况。
        if((head == nullptr) || (head->next == nullptr)){
            return head;
        }
        //交换前两个
        //因为head没有头节点比较特殊
        ListNode *res;
        res = head->next;
        head->next = res->next;
        res->next = head;
        //进行接下来的遍历交换操作
        //res -> head -> ......
        if(head->next != nullptr){
            swapTwo(head,head->next);
        }
        return res;
    }
private:
    void swapTwo(ListNode* pre, ListNode* p){
        //边界条件
        if(( p == nullptr )||( p->next == nullptr )){
            return;
        }
        //交换前两个节点
        ListNode *temp;
        temp = p->next;
        p->next = temp->next;
        temp->next = p;
        pre->next = temp;
        //进行下一次递归
        //pre-->temp -->p
        swapTwo(p,p->next);
    }   
};
```

### 知识点：

用递归可以解决看似很复杂的问题。

### 官方代码：

```
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (head == nullptr || head->next == nullptr) {
            return head;
        }
        ListNode* newHead = head->next;
        head->next = swapPairs(newHead->next);
        newHead->next = head;
        return newHead;
    }
};
```

### 热评：

```
索...索然无味？
```

