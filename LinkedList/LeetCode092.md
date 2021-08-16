### 题目：

给你单链表的头指针 head 和两个整数 left 和 right ，其中 left <= right 。请你反转从位置 left 到位置 right 的链表节点，返回 反转后的链表 。

### 难度：

中等。

### 示例：

```
输入：head = [1,2,3,4,5], left = 2, right = 4
输出：[1,4,3,2,5]
```

### 解题思路：

```
 与其修改节点，不如修改节点对应的值。借助right-left个空间即可完成，但是一趟扫描很难完成。
 还是得在原链表上修改，记录left-1位置的指针，从left到right从新排列链表，实现翻转
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
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        //排除特殊情况
        if( left == right || right == 0){
            return head;
        }
        //临时变量
        ListNode *preLeft = head, *node = nullptr, *newList = nullptr;
        //记录所走步数
        int count = 1;
        //先从left之前断开
        if(left == 1){
            node = head;
        }
        while(left != 1){
            if(count < left - 1){
                preLeft = preLeft->next;
                count++;
            }else if(count == left - 1){
                node = preLeft->next;
                preLeft->next = nullptr;
                count++;
                break;
            }
        }
        //目前：head指向头，preleft指向left-1，preleft->next = nullptr, node指向第left个节点。
        ListNode *temp = nullptr, *toil = nullptr;
        while(true){
            if( count == left){
                newList = node;
                node = node->next;
                newList->next = nullptr;
                toil = newList;
                count++;
            }else if( count < right){
                temp = node;
                node = node->next;
                temp->next = newList;
                newList = temp; 
                count++;
            }else if( count == right){
                //此时left指针指向right位置的节点
                temp = node;
                node = node->next;
                temp->next = newList;
                newList = temp; 

                break;                
            }
        }
        //目前：head指向头，preleft指向left-1，preleft->next = nullptr, left指向第right个节点。
        //newList 指向新的链表的头，toil指向尾
        if( left == 1){
            head = newList;
            toil->next = node;
        }else{
            preLeft->next = newList;
            toil->next = node;
        }
        return head;
    }
};
```

### 知识点：

我的代码分类讨论了left == 1的这种情况。

可以用 **虚拟头节点** 来避免这种讨论。

### 官方代码：

```
class Solution {
private:
    void reverseLinkedList(ListNode *head) {
        // 也可以使用递归反转一个链表
        ListNode *pre = nullptr;
        ListNode *cur = head;

        while (cur != nullptr) {
            ListNode *next = cur->next;
            cur->next = pre;
            pre = cur;
            cur = next;
        }
    }

public:
    ListNode *reverseBetween(ListNode *head, int left, int right) {
        // 因为头节点有可能发生变化，使用虚拟头节点可以避免复杂的分类讨论
        ListNode *dummyNode = new ListNode(-1);
        dummyNode->next = head;

        ListNode *pre = dummyNode;
        // 第 1 步：从虚拟头节点走 left - 1 步，来到 left 节点的前一个节点
        // 建议写在 for 循环里，语义清晰
        for (int i = 0; i < left - 1; i++) {
            pre = pre->next;
        }

        // 第 2 步：从 pre 再走 right - left + 1 步，来到 right 节点
        ListNode *rightNode = pre;
        for (int i = 0; i < right - left + 1; i++) {
            rightNode = rightNode->next;
        }

        // 第 3 步：切断出一个子链表（截取链表）
        ListNode *leftNode = pre->next;
        ListNode *curr = rightNode->next;

        // 注意：切断链接
        pre->next = nullptr;
        rightNode->next = nullptr;

        // 第 4 步：同第 206 题，反转链表的子区间
        reverseLinkedList(leftNode);

        // 第 5 步：接回到原来的链表中
        pre->next = rightNode;
        leftNode->next = curr;
        return dummyNode->next;
    }
};
```

### 热评：

```
“穿针引线”这名字是不错，但我看了半天突然发现这不就是头插法嘛
```

