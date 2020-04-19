---
title: Leetcode24. Swap Nodes in Pairs
tags:
  - 链表
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-07-20 18:06:45
categories: Leetcode题解
---

交换链表中所有相邻节点并返回链表头节点。

<!-- more -->

---

### 24. Swap Nodes in Pairs

Given a linked list, swap every two adjacent nodes and return its head.

**Example:**

```
Given 1->2->3->4, you should return the list as 2->1->4->3.
```

**Note:**

- Your algorithm should use only constant extra space.
- You may **not** modify the values in the list's nodes, only nodes itself may be changed.

---

#### 算法：交换链表节点

操作链表的题目一个很重要的技巧就是在头节点前面添加一个虚拟节点dummy指向头节点，对链表操作完后返回dummy.next。

交换链表相邻节点的操作顺序很重要，如下图所示：

![L2401](/images/L2401.png)

代码如下：

```java
public ListNode swapPairs(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode cur = dummy;
    while(cur.next != null && cur.next.next != null) {
        ListNode pos = cur.next;
        cur.next = pos.next;
        pos.next = pos.next.next;
        cur.next.next = pos;
        cur = cur.next.next;
    }
    return dummy.next;
}
```

