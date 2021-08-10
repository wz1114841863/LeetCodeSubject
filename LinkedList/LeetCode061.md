### 题目：

给你一个链表的头节点 `head` ，旋转链表，将链表每个节点向右移动 `k` 个位置。

### 难度：

中等。

### 示例：

```
输入：head = [1,2,3,4,5], k = 2
输出：[4,5,1,2,3]
```

### 解题思路：

```
如果通过遍历获取链表的长度，进而可以计算k的有效位移量,再利用双指针进行操作。
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
    ListNode* rotateRight(ListNode* head, int k) {
        //处理特情况
        if( head == nullptr || head->next == nullptr || k == 0){
            return head;
        }
        //求长度,最短是2
        int len = 1;
        //头尾指针
        ListNode *toil = head, *newHead = head, *temp = nullptr;
        //遍历链表
        while( toil->next != nullptr){
            len++;
            toil = toil->next;
        }
        //计算有限位移量。
        k = k % len;
        if( k == 0){
            return head;
        }
        //cout << "len:  " << len << endl;
        //cout << "k:  " << k << endl;
        //旋转
        for(int i=0; i< ( len -k -1); i++){
            newHead = newHead->next;
        }
        temp = newHead;
        newHead = newHead->next;
        temp->next = nullptr;
        toil->next = head;

        return newHead;
    }
};
```

### 知识点：

链表题难度比较低。长度的遍历无法避免。

### 官方代码：

```
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (k == 0 || head == nullptr || head->next == nullptr) {
            return head;
        }
        int n = 1;
        ListNode* iter = head;
        while (iter->next != nullptr) {
            iter = iter->next;
            n++;
        }
        int add = n - k % n;
        if (add == n) {
            return head;
        }
        iter->next = head;
        while (add--) {
            iter = iter->next;
        }
        ListNode* ret = iter->next;
        iter->next = nullptr;
        return ret;
    }
};
```

### 热评：

```
别人都擅长dp、dfs、bfs。。。我只擅长链表，哭了
```

