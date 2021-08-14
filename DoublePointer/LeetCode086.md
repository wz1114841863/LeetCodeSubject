### 题目：

给你一个链表的头节点 head 和一个特定值 x ，请你对链表进行分隔，使得所有 小于 x 的节点都出现在 大于或等于 x 的节点之前。

你应当 保留 两个分区中每个节点的初始相对位置。

### 难度：

中等。

### 示例：

```
输入：head = [1,4,3,2,5,2], x = 3
输出：[1,2,2,4,3,5]
```

### 解题思路：

```
一个指针遍历数组.
一个指针记录小于X的末尾节点位置.
    添加一个新的头节点方便下一步处理.
    先处理最前面的本来就小于x的部分,同时也不会导致后面的关系出错.
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
    ListNode* partition(ListNode* head, int x) {
        //处理特殊情况
        if(head == nullptr || head->next == nullptr){
            return head;
        }
        //两个指针
        ListNode *littleNumber = nullptr, *number = nullptr, *toil = nullptr;
        ListNode *temp = nullptr;
        //添加一个头节点,返回时舍去.
        head = new ListNode(x-1, head);
        littleNumber = head;
        toil = head;
        number = toil->next;
        int num;
        //跳过前面小于x的值,不用重新排序
        while(number != nullptr && number->val < x ){
            littleNumber = littleNumber->next;
            toil = toil->next;
            number = number->next;
        }
        while(number != nullptr){
            /*
            cout << "littleNumber:  "  << littleNumber->val;
            cout << "  toil:  "  << toil->val;
            cout << "  number:  " << number->val;
            cout << endl;
            */
            num = number->val;
            if(num < x){
                //修补后面
                temp = number;
                number = temp->next;
                toil->next = number;
                //修补前面
                temp->next = littleNumber->next;
                littleNumber->next = temp;
                littleNumber = littleNumber->next;
                temp = nullptr;
            }else{
                //同时后移
                toil = toil->next;
                number = number->next;
            }

        }
        //delete littleNumber, number, toil, temp;
        return head->next;
    }
};
```

### 知识点：

“被爱的人真幸福。“

### 官方代码：

```
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        ListNode* small = new ListNode(0);
        ListNode* smallHead = small;
        ListNode* large = new ListNode(0);
        ListNode* largeHead = large;
        while (head != nullptr) {
            if (head->val < x) {
                small->next = head;
                small = small->next;
            } else {
                large->next = head;
                large = large->next;
            }
            head = head->next;
        }
        large->next = nullptr;
        small->next = largeHead->next;
        return smallHead->next;
    }
};
```

### 热评：

```
建议官方把难度修改为简单。
```

