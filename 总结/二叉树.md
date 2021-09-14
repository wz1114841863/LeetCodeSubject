# 快慢指针

## 作用

1.仅遍历一次确定链表的中心节点位置

```c++
//示例
//这里的范围使用左闭右开区间
ListNode* getMiddle(ListNode* left, ListNode* right){
    //首先建立快慢指针，起始点都为左指针left。
    Listnode *fast = left, *slow = left;
    while(fast!=right && fast->next!=right){  
        //快指针每次走两步，慢指针每次走一步。
        fast = fast->next;
        fast = fast->next;
        slow = slow->next;
    }
    
    return slow;
}
/*
	根据条件 fast!=right && fast->next!=right
	如果为[left, right)为偶数（不包括right)，则fast指向right时，slow指向中间偏右
	如果为[left, right)为奇数数（不包括right)，则fast指向right的前一个指针时，即fast->next = right，slow指向中间	
*/
```

