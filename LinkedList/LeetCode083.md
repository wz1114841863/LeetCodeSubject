### 题目：

存在一个按升序排列的链表，给你这个链表的头节点 `head` ，请你删除所有重复的元素，使每个元素 **只出现一次** 。

返回同样按升序排列的结果链表。

### 难度：

简单

### 示例：

```
输入：head = [1,1,2,3,3]
输出：[1,2,3]
```

### 解题思路：

```
 使用双指针：
    如果first节点值和second节点值相同，删除second节点，secon节点后移。不同，first后移，secon的节点后移，
```

### 代码实现：

```c++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        //排除特殊情况
        if(head == nullptr || head->next == nullptr){
            return head;
        }
        //遍历节点
        ListNode *first = head, *second = head->next;
        while(second != nullptr){
            if(first->val != second->val){
                first = first->next;
            }else{
                first->next = second->next;
            }
            second = second->next;
        }

        return head;
    }
};
```

### 知识点：

经典双指针。

### 官方代码：

```
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head) {
            return head;
        }

        ListNode* cur = head;
        while (cur->next) {
            if (cur->val == cur->next->val) {
                cur->next = cur->next->next;
            }
            else {
                cur = cur->next;
            }
        }

        return head;
    }
};
```

### 热评：

```
先 难 后 易？化 繁 为 简 ？ 这波您在第几层呢？（ ^_^ ）
```

```
我懂了，这波是深入♂浅出
```

