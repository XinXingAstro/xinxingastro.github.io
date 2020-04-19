---
title: Leetcode206. Reverse Linked List
tags:
  - 链表
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-06 16:53:13
categories: Leetcode题解
---

反转单链表。

剑指Offer面试题24: 反转链表。

<!-- more -->

---

### 206. Reverse Linked List

Reverse a singly linked list.

**Example:**

```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```

**Follow up:**

A linked list can be reversed either iteratively or recursively. Could you implement both?

---

#### 算法：操作三个指针逐个几点翻转

这道题的难点是指针操作，异常处理。

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode pos = null;
    ListNode cur = head;
    ListNode pre = head.next;
    while (pre != null) {
        cur.next = pos;
        pos = cur;
        cur = pre;
        pre = pre.next;
    }
    cur.next = pos;
    return cur;
}
```

时间复杂度：O(n)

空间复杂度：O(1)