---
title: Leetcode2. Add Two Numbers
tags:
  - 链表
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-05 15:58:21
categories: Leetcode题解
---

用两个链表代表两个数字，两链表相加得到一个新的链表。

<!-- more -->

---

### 2. Add Two Numbers

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example**

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

---

这道题我们需要创建两个指针，一个指针指向最后生成链表的头部，另一个指针要循环创建链表节点。

技巧是：为了能让循环内的语句循环执行，我们需要在头节点前面新建一个节点作为启动节点。

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    if (l1 == null || l2 == null) return null;
    ListNode head = new ListNode(0); // 我们新建一个启动节点，既返回头节点前面的一个节点
    ListNode pNode = head;
    int carry = 0, sum = 0;
    while (l1 != null || l2 != null || carry != 0) {
        pNode.next = new ListNode(0); // 如果上面三个条件有一个成立我们就需要新建一个节点
        sum = (l1 == null ? 0 : l1.val) + (l2 == null ? 0 : l2.val) + carry;
        carry = sum >= 10 ? 1 : 0;
        sum = sum >= 10 ? (sum - 10) : sum;
        pNode = pNode.next;
        pNode.val = sum;
        if (l1 != null) l1 = l1.next;
        if (l2 != null) l2 = l2.next;
    }
    return head.next;
}
```

