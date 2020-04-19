---
title: Leetcode21. Merge Two Sorted Lists
tags:
  - 链表
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-05 22:09:58
categories: Leetcode题解
---

合并两个排序链表。

剑指Offer面试题25: 合并两个排序的链表。

<!-- more -->

---

### 21. Merge Two Sorted Lists

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

**Example:**

```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

---

**对于链表的操作，为了处理头节点使循环变的一致，我们通常会在链表前面定义一个虚假节点指向链表头节点。**

对于这个问题，我们可以使用归并排序中的合并算法，只不过在链表上合并时，会涉及到很多指针操作，另外需要定义头节点前的虚假节点。

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(-1); // 设置头节点前的虚假节点
    ListNode l3 = dummy;
    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            l3.next = l1;
            l1 = l1.next;
        } else {
            l3.next = l2;
            l2 = l2.next;
        }
        l3 = l3.next;
    }
    if (l1 != null) l3.next = l1;
    if (l2 != null) l3.next = l2;
    return dummy.next;
}
```

