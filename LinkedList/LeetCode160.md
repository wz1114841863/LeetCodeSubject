### 题目：

给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。

题目数据 保证 整个链式结构中不存在环。

注意，函数返回结果后，链表必须 保持其原始结构 。

### 难度：

简单。

### 示例：

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Intersected at '8'
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

### 解题思路：

```c++
暴力解法：
    1.直接遍历链表A，利用哈希表存储所有的节点
    2.遍历链表，查找当前节点是否在在哈希表中
        如果遍历结束，依旧没查找到，返回nullptr
        如果查找到，说明当前节点即为第一个交叉节点。
改进解法1：
    1.遍历两个链表，获取链表的长度m,n。
    2.较长的链表先走m-n步。
    3.然后两个链表同时开始走，直到结束。
        结束前相遇，返回指针
        结束前未相遇返回nullptr。
改进解法2：
    1.利用数学关系：
        skipA + commonNode + skipB = skipB + commonNode + skipA.
    2.
        指针A先遍历链表A，遍历结束后从链表B继续开始第二次遍历。
        指针B先遍历链表B，遍历结束后从链表A继续开始第二次遍历。
    3.相遇方式
        如果链表A与链表B长度相同，则指针A、B第一遍历即会相遇。
        如果长度不相同，则会在第二次遍历相遇。
    4.如果没有相交节点，第二次遍历结束时返回nullptr。
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
 //改进解法1
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        //排除特殊情况
        if(headA == nullptr || headA == nullptr) return nullptr;
        //获取长度
        int lenA = 0, lenB = 0;
        ListNode *nodeA = headA, *nodeB = headB;
        while(nodeA){
            lenA++;
            nodeA = nodeA->next;
        }
        while(nodeB){
            lenB++;
            nodeB = nodeB->next;
        }
        //获取其中较长的链表
        int maxLen = lenA > lenB ? lenA : lenB;
        int minLen = lenA > lenB ? lenB : lenA;
        ListNode *maxList = lenA > lenB ? headA : headB;
        ListNode *minList = lenA > lenB ? headB : headA;
        //长度长的先走 max-min 步
        int diff = maxLen - minLen;
        while(diff--){
            maxList = maxList->next;
        }
        //比较指针
        while(minLen--){
            if(maxList == minList){
                return maxList;
            }
            maxList = maxList->next;
            minList = minList->next;
        }
        return nullptr;
    }
};
*/

//改进解法2
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        //排除特殊情况
        if(headA == nullptr || headA == nullptr) return nullptr;
        //开始遍历
        ListNode *nodeA = headA, *nodeB = headB;
        while(nodeA != nodeB){
            nodeA = (nodeA == nullptr) ? headB : nodeA->next;
            nodeB = (nodeB == nullptr) ? headA : nodeB->next;
        } 
        return nodeA;
    }
};
```

### 知识点：

利用数学关系求解相遇问题。

### 官方代码：

```c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if (headA == nullptr || headB == nullptr) {
            return nullptr;
        }
        ListNode *pA = headA, *pB = headB;
        while (pA != pB) {
            pA = pA == nullptr ? headB : pA->next;
            pB = pB == nullptr ? headA : pB->next;
        }
        return pA;
    }
};
```

### 热评：

```
错的人就算走过了对方的路也还是会错过😔 这题我希望大家都返回true
```

