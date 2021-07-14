### 题目：

​	给你两个非空的链表，表示两个非负的整数。它们每位数字都是按照逆序的方式存储的，并且每个节点只能存储一位数字。请你将两个数相加，并以相同形式返回一个表示和的链表。

### 难度：

​	中等。

### 示例：

```
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```

### 解题思路：

1.不能将链表转换为数字后计算出结果后再转换回链表，过于浪费时间。

2.考虑进位情况和链表长度，逐位处理。

```
伪代码：
1.判断输入的链表指针非空，若为空则直接返回另一指针。
2.初始化，新建链表节点。
3.循环处理，取值，开辟新节点，赋值，考虑进位等。
4.返回pre_head->next
注意：
为了处理⽅法统⼀，可以先建⽴⼀个虚拟头结点，这个虚拟头结点的 Next 指向真正的 head，这样head 不需要单独处理，直接 while 循环即可。
判断循环终⽌的条件不⽤是 li.next ！= nullptr，这样最后⼀位还需要额外计算，循环终⽌条件应该是 li != nullptr。
需要充分考虑各种可能出现的情况。
```

### 代码实现：

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
**/
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        //判断链表指针l1,l2非空。
        if(l1 == nullptr && l2 == nullptr){
            return nullptr;
        }else if(l1 == nullptr && l2 != nullptr){
            return l2;
        }else if(l1 != nullptr && l2 == nullptr){
            return l1;
        }
        //初始化
        struct ListNode *pre_head = nullptr, *tail = nullptr;
        struct ListNode *temp = nullptr;
        int flag = 0; //1:进位 0：无进位。
        //循环判断，任何一个不为空就要继续相加，短的链表高位补零。
         //开辟一个pre_head头节点，pre_head->next 为head。
        pre_head = tail = new ListNode(0);
        while(l1 || l2){
            //取值,使用三目运算符,非空取值，为空取0。
            int n1 = l1 ? l1->val : 0;
            int n2 = l2 ? l2->val : 0;
            //相加，同时考虑进位标志
            int sum = n1 + n2 + flag;
            //节点保存值 < 10 
            int value = sum % 10;
            //flag 
            //falg = sum >= 10 ? 1 : 0;
            flag = sum / 10;
            //添加节点。
            tail->next = new ListNode(value);
            tail = tail->next;
            //节点移动
            if(l1){
                temp = l1;
                l1 = l1->next;
                delete temp;
            }
            if(l2){
                temp = l2;
                l2 = l2->next;
                delete temp;    
            }
            temp = nullptr;
        }

        if (flag> 0) {

            tail->next = new ListNode(flag);

        }
        return pre_head->next;
    }
};
```

### 知识点：

#### 1. null与nullptr

C++中的NULL定义如下：

```c++
/* Define NULL pointer value */
#ifndef NULL
    #ifdef __cplusplus
        #define NULL    0
    #else  /* __cplusplus */
        #define NULL    ((void *)0)
    #endif  /* __cplusplus */
#endif  /* NULL */
```

​		可以看到将NULL定义为零。因为C++中不能将void类型的指针隐式转换成其他指针类型，所以用(void*)对其他类型指针赋初值是不行的。既然(void*)0不能起到空指针的作用，不如干脆将NULL定义为0，引入0来表示空指针，可以对各种类型的指针进行赋值。

​		因此C++11加入了nullptr，可以保证在任何情况下都代表空指针，而不会出现上述的情况，因此，建议以后还是都用nullptr替代NULL吧，而NULL就当做0使用。

​		此外，NULL并不是C++中的关键字，而nullptr是C++中的关键字。

#### 2. new 和 delete

并不像C语言那样使用malloc()来开拓空间，而使用new来创建新对象。

对象使用完之后需要释放空间，直接使用delete来释放即可。

释放完之后应该将空指针设置为nullptr。

#### 3.结构体

```
struct ListNode {
	int val;
	ListNode *next;
	ListNode() : val(0), next(nullptr) {}
	ListNode(int x) : val(x), next(nullptr) {}
	ListNode(int x, ListNode *next) : val(x), next(next) {}
};
```

与C语言明显不同。结构体中不仅包含int类型、Listnode类型，还包含三种指针初始化方法。

### 官方解答：

```

class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *head = nullptr, *tail = nullptr;
        int carry = 0;
        while (l1 || l2) {
            int n1 = l1 ? l1->val: 0;
            int n2 = l2 ? l2->val: 0;
            int sum = n1 + n2 + carry;
            if (!head) {
                head = tail = new ListNode(sum % 10);
            } else {
                tail->next = new ListNode(sum % 10);
                tail = tail->next;
            }
            carry = sum / 10;
            if (l1) {
                l1 = l1->next;
            }
            if (l2) {
                l2 = l2->next;
            }
        }
        if (carry > 0) {
            tail->next = new ListNode(carry);
        }
        return head;
    }
};
```

### 热评：

```
一顿操作猛如虎，一看击败百分五。
```

