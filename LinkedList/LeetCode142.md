### 题目：

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。

说明：不允许修改给定的链表。

进阶：

你是否可以使用 O(1) 空间解决此题？

### 难度：

中等。

### 示例：

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

### 解题思路：

```c++
使用哈希表存储节点信息：
    每遍历一个节点就查找其是否在哈希表里，如果是就说明这就是入环的第一个节点，返回该节点。
    如果不是就加入哈希表中，直到查询到空指针。
    时间复杂度O(N)
    空间复杂度O(N)
使用快慢指针：
    1.判断是否有环。
    2.根据数学关系找出入环的第一节点。
```

### 代码实现：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
 //哈希表法
/*
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        //创建哈希表
        unordered_set<ListNode*> node;
        ListNode* cur = head;
        while(cur){
            if(find(node.begin(), node.end(), cur) != node.end()){
                return cur;
            }else{
                node.insert(cur);
            }
            cur = cur->next;
        }
        return nullptr;
    }
};
*/

class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        //快慢指针
        ListNode *fast = head, *slow = head;
        while(fast && fast->next){
            slow = slow->next;
            fast = fast->next->next;
            if(fast == slow){
                ListNode *pre = head;
                while(pre != slow){
                    pre = pre->next;
                    slow = slow->next;
                }
                return pre;
            }
        }
        return nullptr;
    }
};
```

### 知识点：

slow = a + b

fast = a + b  + n(b+c)

fast = 2*slow

a = c + (n-1)*(b+c)

### 官方代码：

```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *slow = head, *fast = head;
        while (fast != nullptr) {
            slow = slow->next;
            if (fast->next == nullptr) {
                return nullptr;
            }
            fast = fast->next->next;
            if (fast == slow) {
                ListNode *ptr = head;
                while (ptr != slow) {
                    ptr = ptr->next;
                    slow = slow->next;
                }
                return ptr;
            }
        }
        return nullptr;
    }
};
```

### 热评：

```
//解释一下为什么慢指针入环第一圈没走完的时候就会和快指针相遇
设 环的长度为A,慢指针在入环的时候快指针在环中的位置B(取值范围0到A-1),
当快慢指针相遇时 [慢指针在环中走了C] ,有
    C % A = ( B + 2C) % A,等价于 
    An + C = B + 2C,合并得
    C = An - B
    当 n=1 时 , 0 <= C < A
故 慢指针在第一圈必定能和快指针相遇
```

