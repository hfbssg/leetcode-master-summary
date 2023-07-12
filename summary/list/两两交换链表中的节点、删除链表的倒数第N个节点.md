#### 一、两两交换链表中的节点

##### 题意：

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

![1689131023003](.\images\1689131023003.png)

##### 思路：

使用虚拟头结点求解。

##### c++代码：

```c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead = new ListNode(0); // 设置一个虚拟头结点
        dummyHead->next = head; // 将虚拟头结点指向head，这样方面后面做删除操作
        ListNode* cur = dummyHead;
        while(cur->next != nullptr && cur->next->next != nullptr) {
            ListNode* tmp = cur->next; // 记录临时节点
            ListNode* tmp1 = cur->next->next->next; // 记录临时节点

            cur->next = cur->next->next;    // 步骤一
            cur->next->next = tmp;          // 步骤二
            cur->next->next->next = tmp1;   // 步骤三
     
            cur = cur->next->next; // cur移动两位，准备下一轮交换
        }
        return dummyHead->next;
    }

};
```

##### 心得：

1、临时指针tmp进行节点交换的操作。

2、

![1689130946057](.\images\1689130946057.png)

tmp1特别重要，不能忽略，目的是确保在进行节点交换时，不丢失链表中后续的节点，并正确地连接交换后的节点。

#### 二、删除链表的倒数第N个节点

##### 题意：

给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

如：

![1689131099243](.\images\1689131099243.png)

输入：head = [1,2,3,4,5], n = 2 输出：[1,2,3,5] 示例 2：

输入：head = [1], n = 1 输出：[] 示例 3：

输入：head = [1,2], n = 1 输出：[1]

##### 思路：

使用双指针的方法，分别用slow表示慢指针、fast表示快指针。

##### c++代码：

```c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* slow = dummyHead;
        ListNode* fast = dummyHead;
        while(n-- && fast != NULL) {
            fast = fast->next;
        }
        fast = fast->next; // fast再提前走一步，因为需要让slow指向删除节点的上一个节点
        while (fast != NULL) {
            fast = fast->next;
            slow = slow->next;
        }
        slow->next = slow->next->next; 
        

        // ListNode *tmp = slow->next;  C++释放内存的逻辑
        // slow->next = tmp->next;
        // delete nth;
        
        return dummyHead->next;
    }

};
```

##### 心得：

![1689131219819](.\images\1689131219819.png)

注意这里的fast在走了n步之后还要再走一步；是为了始终让slow指向删除节点的上一个节点，方便操作后续的节点。
