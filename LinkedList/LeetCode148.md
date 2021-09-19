### 题目：

给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。

进阶：

你可以在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序吗？

### 难度：

中等。

### 示例：

```
输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]
```

### 解题思路：

```c++
    归并排序：
        时间复杂度为 O(nlogn).
        归并算法需要找到链表的中点，可以考虑使用快慢指针。
        分治时可以考虑使用递归来实现。
    可以使用递归，但空间复杂度不为常数级。
    改进：
        使用从下往上的归并排序。原地修改排序。用时间换空间。
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
 //递归实现归并排序
 /*
class Solution {
public:
    //返回已经排序好的链表
    ListNode* mergeSort(ListNode* head){
        //返回条件
        if(head->next == nullptr) return head;
        //分治
        //利用快慢指针查找中点
        ListNode *fast = head->next, *slow = head, *pre = nullptr;
        while(fast!=nullptr && fast->next != nullptr){
            fast = fast->next;
            fast = fast->next;
            slow = slow->next;
        }
        pre = slow;
        slow = slow->next;
        pre->next = nullptr;
        //分治
        ListNode *left = new ListNode(0), *right = new ListNode(0);
        left->next = mergeSort(head);
        right->next = mergeSort(slow);
        //排序
        ListNode *res = new ListNode(0), *resToil = res;
        while(left->next!=nullptr  && right->next!=nullptr){
            if(left->next->val <= right->next->val){
                resToil->next = left->next;
                left->next = left->next->next;
            }else{
                resToil->next = right->next;
                right->next = right->next->next;
            }
            resToil = resToil->next;
        }
        resToil->next = nullptr;
        if(left->next != nullptr){
            resToil->next = left->next;
        }else if(right->next != nullptr){
            resToil->next = right->next;
        }
        return res->next;
    }
    ListNode* sortList(ListNode* head) {
        //排除特殊情况
        if(head == nullptr || head->next == nullptr) return head;
        //归并排序
        ListNode* res = mergeSort(head);
        return res;
    }
};
*/
//迭代实现归并排序
class Solution {
public:
    //双路合并函数
    ListNode* merge(ListNode* L1, ListNode* L2){
        //返回结果
        ListNode *res = new ListNode(-1), *toil = res;
        while(L1 && L2){
            if(L1->val < L2->val){
                toil->next = L1;
                toil = toil->next;
                L1 = L1->next;
            }else{
                toil->next = L2;
                toil = toil->next;
                L2 = L2->next;
            }
        }
        //其中一个链表为空时
        toil->next = L1 ? L1 : L2;

        return res->next;
    }
    //spilt操作
    //返回指向L1的第n+1个节点指针
    ListNode* cut(ListNode* L1, int n){
        ListNode* p = L1;
        //判断链表的长度是否大于等于n，并获取第n-1个节点的指针。
        while( --n && p){
            p = p->next;
        }
        if(!p) return nullptr;
        //处理第n-1个节点的末尾，添加nullptr。
        auto res = p->next;
        p->next = nullptr;
        return res;
    }
    ListNode* sortList(ListNode* head) {
        //使用dummyNode
        ListNode *dummyNode = new ListNode(-1, head);
        ListNode *p = head;
        int len = 0;
        //获取链表长度
        while(p){
            ++len;
            p = p->next;
        }
        //归并排序
        for(int size = 1; size < len; size *= 2){
            ListNode* cur = dummyNode->next;
            ListNode* tail = dummyNode;
            while(cur){
                //切割
                auto left = cur;
                auto right = cut(left, size);
                cur = cut(right, size);
                //合并
                tail->next = merge(left, right);
                while(tail->next){
                    tail = tail->next;
                }
            }

        }

        return dummyNode->next;
    }
};
```

### 知识点：

链表知识点：

​	1.哑巴节点

​	2.快慢指针

​	3.merge排序

​	4.cut切除

### 官方代码：

```c++
class Solution {
    public ListNode sortList(ListNode head) {
        if (head == null) {
            return head;
        }
        int length = 0;
        ListNode node = head;
        while (node != null) {
            length++;
            node = node.next;
        }
        ListNode dummyHead = new ListNode(0, head);
        for (int subLength = 1; subLength < length; subLength <<= 1) {
            ListNode prev = dummyHead, curr = dummyHead.next;
            while (curr != null) {
                ListNode head1 = curr;
                for (int i = 1; i < subLength && curr.next != null; i++) {
                    curr = curr.next;
                }
                ListNode head2 = curr.next;
                curr.next = null;
                curr = head2;
                for (int i = 1; i < subLength && curr != null && curr.next != null; i++) {
                    curr = curr.next;
                }
                ListNode next = null;
                if (curr != null) {
                    next = curr.next;
                    curr.next = null;
                }
                ListNode merged = merge(head1, head2);
                prev.next = merged;
                while (prev.next != null) {
                    prev = prev.next;
                }
                curr = next;
            }
        }
        return dummyHead.next;
    }

    public ListNode merge(ListNode head1, ListNode head2) {
        ListNode dummyHead = new ListNode(0);
        ListNode temp = dummyHead, temp1 = head1, temp2 = head2;
        while (temp1 != null && temp2 != null) {
            if (temp1.val <= temp2.val) {
                temp.next = temp1;
                temp1 = temp1.next;
            } else {
                temp.next = temp2;
                temp2 = temp2.next;
            }
            temp = temp.next;
        }
        if (temp1 != null) {
            temp.next = temp1;
        } else if (temp2 != null) {
            temp.next = temp2;
        }
        return dummyHead.next;
    }
}
```

### 热评：

```
原谅我毫不犹豫复制了昨天的写法
```

