### 题目：

给你一个长度为 n 的链表，每个节点包含一个额外增加的随机指针 random ，该指针可以指向链表中的任何节点或空节点。

构造这个链表的 深拷贝。 深拷贝应该正好由 n 个 全新 节点组成，其中每个新节点的值都设为其对应的原节点的值。新节点的 next 指针和 random 指针也都应指向复制链表中的新节点，并使原链表和复制链表中的这些指针能够表示相同的链表状态。复制链表中的指针都不应指向原链表中的节点 。

例如，如果原链表中有 X 和 Y 两个节点，其中 X.random --> Y 。那么在复制链表中对应的两个节点 x 和 y ，同样有 x.random --> y 。

返回复制链表的头节点。

用一个由 n 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 [val, random_index] 表示：

val：一个表示 Node.val 的整数。
random_index：随机指针指向的节点索引（范围从 0 到 n-1）；如果不指向任何节点，则为  null 。
你的代码 只 接受原链表的头节点 head 作为传入参数。

### 难度：

中等。

### 示例：

```
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
```

### 解题思路：

```c++
    1.处理节点：
        复制当前节点，将原链表变为
            1->1'->2->2'->...
    2.处理random节点
    3.分离两个链表

```

### 代码实现：

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution {
public:
    Node* copyRandomList(Node* head) {
        //排除特殊情况
        if(head == nullptr) return nullptr;
        //复制节点
        Node *cur = head;
        while(cur){
            Node *temp = new Node(cur->val);
            temp->next = cur->next;
            cur->next = temp;
            cur = temp->next;
        }
        //处理random节点
        cur = head;
        while(cur){
            if(cur->random == nullptr){
                cur->next->random = nullptr;
            }else{
                cur->next->random = cur->random->next;   
            }
            
            cur = cur->next->next;
        }
        //分割链表
        cur = head;
        Node *newHead = new Node(-1);
        Node *newCur = newHead;
        while(cur){
            newCur->next = cur->next;
            cur->next = cur->next->next;
            newCur = newCur->next;
            newCur->next = nullptr;
            cur = cur->next;
        }

        return newHead->next;

    }
};
```

### 知识点：

还可以利用哈希表存储节点。

### 官方代码：

```c++
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (head == nullptr) {
            return nullptr;
        }
        for (Node* node = head; node != nullptr; node = node->next->next) {
            Node* nodeNew = new Node(node->val);
            nodeNew->next = node->next;
            node->next = nodeNew;
        }
        for (Node* node = head; node != nullptr; node = node->next->next) {
            Node* nodeNew = node->next;
            nodeNew->random = (node->random != nullptr) ? node->random->next : nullptr;
        }
        Node* headNew = head->next;
        for (Node* node = head; node != nullptr; node = node->next) {
            Node* nodeNew = node->next;
            node->next = node->next->next;
            nodeNew->next = (nodeNew->next != nullptr) ? nodeNew->next->next : nullptr;
        }
        return headNew;
    }
};
```

### 热评：

```
public Node copyRandomList(Node head) {
    if(head == null){
        return null;
    }
    Node cur = head;
    HashMap<Node,Node> map = new HashMap<>();
    while(cur!=null){
        map.put(cur,new Node(cur.val));
        cur = cur.next;
    }
    cur=head;
    while(cur!=null){
        map.get(cur).next=map.get(cur.next);
        map.get(cur).random=map.get(cur.random);
        cur=cur.next;
    }
    return map.get(head);
}
/*
第一遍遍历复制所有节点。
第二遍处理所有的指针关系，就不用再分离链表了。
*/
```

