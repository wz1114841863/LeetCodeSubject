### 题目：

存在一个按升序排列的链表，给你这个链表的头节点 head ，请你删除链表中所有存在数字重复情况的节点，只保留原始链表中 没有重复出现 的数字。

返回同样按升序排列的结果链表。

### 难度：

中等。

### 示例：

```
输入：head = [1,2,3,3,4,4,5]
输出：[1,2,5]
```

### 解题思路：

```
    创建一个新的列表储存返回结果。
    使用两个头尾指针，
    头指针指向前一节点，尾指针遍历。
    如果结果不同，头指针指向的位置添加进入链表，如果相同，记录flag， 尾指针后移。
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
    ListNode* deleteDuplicates(ListNode* head) {
        //排除特殊情况
        if(head == nullptr || head->next == nullptr){
            return head;
        }
        //返回结果
        ListNode *res = nullptr, *toil = nullptr;
        //双指针
        ListNode *first, *second;
        //遍历
        int flag=0, temp;
        first = head;
        second = head->next;
        while(second != nullptr){
            
            if(first->val != second->val){
                if(flag == 1){
                    flag = 0;
                }else{
                    temp = first->val;
                    if(res == nullptr){
                        res = new ListNode(temp);
                        toil = res;
                    }else{
                        toil->next = new ListNode(temp);
                        toil = toil->next;
                    }
                }
                first = second;
            }else if(first->val == second->val){
                flag = 1;
            }
            
            second = second->next;
        }
        //处理最后一次first和flag。
        if(flag == 0){
            temp = first->val;
            if(res == nullptr){
                res = new ListNode(temp);
                toil = res;
            }else{
                toil->next = new ListNode(temp);
                toil = toil->next;
            }
        }
        return res;
         
    }
};
```

### 知识点：

调试过程中出现了莫名其妙的bug，res初始化后未赋值，但直接返回时等于 head.

迷惑。

最后将res 赋值为nullptr;

### 官方代码：

```
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head) {
            return head;
        }
        
        ListNode* dummy = new ListNode(0, head);

        ListNode* cur = dummy;
        while (cur->next && cur->next->next) {
            if (cur->next->val == cur->next->next->val) {
                int x = cur->next->val;
                while (cur->next && cur->next->val == x) {
                    cur->next = cur->next->next;
                }
            }
            else {
                cur = cur->next;
            }
        }

        return dummy->next;
    }
};
```

### 热评：

```
链表的题通常需要注意两点：
1.舍得用变量，千万别想着节省变量，否则容易被逻辑绕晕
2.head 有可能需要改动时，先增加一个 假head，返回的时候直接取 假head.next，这样就不需要为修改 head 增加一大堆逻辑了。
```

