---
title: Leetcode141. Linked List Cycle
tags:
  - 链表
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-06 13:51:24
categories: Leetcode题解
---

判断链表中是否存在环，一个非常基础的算法，很多地方用得到。

<!-- more -->

---

### 141. Linked List Cycle

Given a linked list, determine if it has a cycle in it.

Follow up:
Can you solve it without using extra space?

---

#### 算法：快慢指针法

设置一个快指针每次移动两个节点，设置一个慢指针每次移动一个节点，如果在运行过程中快指针能遇到慢指针，则说明链表中存在环，如果快指针到达链表末尾，则说明链表中不存在环。

这个算法的难点是边界条件的处理，既判断快指针到没到链表末尾。

```java
public boolean hasCycle(ListNode head) {
    if (head == null) return false;
    ListNode fast = head;
    ListNode slow = head;
    while (fast != null && fast.next != null) {
        fast = fast.next.next;
        slow = slow.next;
        if (fast == slow) return true;
    }
    return false;
}
```

