### 题目：

给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false 。

 

进阶：

你能用 O(1)（即，常量）内存解决此问题吗？

### 难度：

简单。

### 示例：

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

### 解题思路：

```
    1.利用哈希表去存地址，查找到说明就是环。
    2.快慢指针，快指针一次走两步，慢指针一次走一步，
        如果是环，快指针会追上慢指针，否则不是环。
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
 /*
    空间复杂度为O(n)
 */
 /*
class Solution {
public:
    bool hasCycle(ListNode *head) {
        //排除特殊情况
        if(head == nullptr){
            return false;
        }
        //哈希表
        unordered_set<ListNode*> s1;
        s1.insert(head);
        ListNode *temp = head->next;
        while(1){
            if( temp == nullptr){
                return false;
            }
            if(s1.count(temp)){
                return true;
            }
            s1.insert(temp);
            temp = temp->next;
            
        }
    }
};
*/
/*
    空间复杂度O(1)
    快慢指针
*/
class Solution {
public:
    bool hasCycle(ListNode *head) {
        //排除特殊情况
        if(head == nullptr || head->next == nullptr){
            return false;
        }
        //快慢指针
        ListNode *slow = head, *fast = head->next;
        while(1){
            if(fast->next == nullptr || fast->next->next == nullptr){
                return false;
            }
            if(fast == slow){
                return true;
            }
            fast = fast->next->next;
            slow = slow->next;
        }
    }
};
```

### 知识点：

这里应该考虑到的是：

​	如果是环，fast就一定会与slow相撞。而不是恰好总会错开。

### 官方代码：

```
class Solution {
public:
    bool hasCycle(ListNode* head) {
        if (head == nullptr || head->next == nullptr) {
            return false;
        }
        ListNode* slow = head;
        ListNode* fast = head->next;
        while (slow != fast) {
            if (fast == nullptr || fast->next == nullptr) {
                return false;
            }
            slow = slow->next;
            fast = fast->next->next;
        }
        return true;
    }
};
```

### 热评：

```
//只有我的代码这么奇葩吗=-=
class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        while head:
            if head.val == 'bjfuvth':
                return True
            else:
                head.val = 'bjfuvth'
            head = head.next
        return False
```

```
//我帮你们测试好了，样例最多的有8029个数据
public class Solution {
    public boolean hasCycle(ListNode head) {
        int count=8029;
        
        while(head!=null&&count>0){
            head=head.next;
            count--;
        }
        if(head==null)
            return false;
            
        return true;
        
        
    }
}
```

