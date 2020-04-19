---
title: Leetcode148. Sort List
tags:
  - 链表
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-06 15:18:21
categories: Leetcode题解
---

对链表节点排序，要求时间复杂度为O(nlogn)，空间复杂的为O(1)。

<!-- more -->

---

### 148. Sort List

Sort a linked list in *O*(*n* log *n*) time using constant space complexity.

**Example 1:**

```
Input: 4->2->1->3
Output: 1->2->3->4
```

**Example 2:**

```
Input: -1->5->3->4->0
Output: -1->0->3->4->5
```

---

#### 算法：归并排序

在链表上进行排序，由于题目要求时间复杂度O(nlogn)，所以使用归并排序算法。

```java
public ListNode sortList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode pre = head;
    ListNode slow = head;
    ListNode fast = head;
    while (fast != null && fast.next != null) {
        pre = slow;
        slow = slow.next;
        fast = fast.next.next;
    }
    fast = slow;
    pre.next = null;
    ListNode l1 = sortList(head);
    ListNode l2 = sortList(fast);
    return merge(l1, l2);
}
public ListNode merge(ListNode l1, ListNode l2) {
    if (l1 == null) return l2;
    if (l2 == null) return l1;
    ListNode head = new ListNode(Integer.MIN_VALUE);
    ListNode pNode = head;
    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            pNode.next = l1;
            pNode = pNode.next;
            l1 = l1.next;
        } else {
            pNode.next = l2;
            pNode = pNode.next;
            l2 = l2.next;
        }
    }
    if (l1 != null) pNode.next = l1;
    if (l2 != null) pNode.next = l2;
    return head.next;
}
```

时间复杂度：O(nlogn)

空间复杂度：O(1)