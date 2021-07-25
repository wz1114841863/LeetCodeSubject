### 题目：

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

### 难度：

简单。

### 示例：

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

### 解题思路：

```
初始思路：
新建空链表用来存放最终结果。
依次比较两个链表的最小值（表头），更小的入新新链表。
依次遍历。
最终剩余的链表直接拼接到新链表尾部。
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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        //先考虑特殊情况:有任意一个为空列表，返回另一个
        if( l1 == nullptr){
            //cout << "l2" << endl;
            return l2;
        }else if( l2 == nullptr){
            //cout << "l1" << endl;
            return l1;
        }
        //最终返回结果
        ListNode *result = new ListNode(0);
        ListNode *temp = nullptr;
        temp = result;
        //遍历两个链表:真值条件为任意一个链表不为空。
        while((l1 != nullptr) && (l2 != nullptr)){
            if( l1->val <= l2->val){
                //cout << "l1_val" << l1->val << endl;
                temp->next = l1;
                l1 = l1->next;
                temp = temp->next;
                temp->next = nullptr;
            }else{
                //cout << "l2_val" << l2->val << endl;
                temp->next = l2;
                l2 = l2->next;
                temp = temp->next;
                temp->next = nullptr;
            }
        }
        //判断l1,l2是否为空。
        if(l1 != nullptr){
            temp->next = l1;
        }
        if(l2 != nullptr){
            temp->next = l2;
        }

        return result->next;
    }
};
```

### 知识点：

还可以使用递归的思想去遍历两个链表。

### 官方代码：

```
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        } else if (l2 == null) {
            return l1;
        } else if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }

    }
}
```

### 热评：

```
我可能是leetcode中最菜的一个。。。。。。
```

