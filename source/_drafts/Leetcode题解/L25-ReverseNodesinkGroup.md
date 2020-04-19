---
title: Leetcode25. Reverse Nodes in k-Group
tags:
  - 链表
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-07-20 20:42:43
categories: Leetcode题解
---

在链表中翻转指定长度k的一段链表。

<!-- more -->

---

### 25. Reverse Nodes in k-Group

Given a linked list, reverse the nodes of a linked list *k* at a time and return its modified list.

*k* is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of *k* then left-out nodes in the end should remain as it is.


**Example:**

Given this linked list: `1->2->3->4->5`

For *k* = 2, you should return: `2->1->4->3->5`

For *k* = 3, you should return: `3->2->1->4->5`

**Note:**

- Only constant extra memory is allowed.
- You may not alter the values in the list's nodes, only nodes itself may be changed.

---

#### 算法：翻转链表

本题的难点是指针操作，我们开始还是要创建一个dummy节点指向头节点，在操作结束后然后dummy.next。操作的步骤如下所示：

初始状态时，dummy = cur = end

1）我使用一个循环k次的for循环往后移动end指针，如果end不为null，并且循环了k次，那么我们就要对从cur.next到end这部分链表进行翻转。

2）取出cur.next节点，然后插入到end节点后面，再取出cur.next节点插入到end节点后面，循环k-1次，就可以将这段链表翻转，单个操作如下图所示：

![L2501](/images/L2501.png)

假设k=4，经过上面的步骤后，我们就讲1号节点插入到了4号节点的前面。

3）经过上面链表翻转以后，我们将cur和end指针放到5号节点前面一个节点处，继续执行步骤1），如果end=null，我们直接跳出循环返回dummy.next。

```java
public ListNode reverseKGroup(ListNode head, int k) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode cur = dummy, end = dummy;
    while(true) {
        int i = 0;
        for (;i < k; i++) {
            end = end.next;
            if (end == null) break;
        }
        if(i == k) {
            ListNode pos = cur.next;
            for (int j = 0; j < k-1; j++) {
                ListNode pNode = cur.next;
                cur.next = cur.next.next;
                pNode.next = end.next;
                end.next = pNode;
            }
            cur = pos;
            end = cur;
        } else
            break;
    }
    return dummy.next;
}
```

时间复杂度：O(n).

空间复杂度：O(1).