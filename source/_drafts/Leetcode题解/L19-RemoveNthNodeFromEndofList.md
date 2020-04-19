---
title: Leetcode19. Remove Nth Node From End of List
tags:
  - 链表
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-05 18:37:41
categories: Leetcode题解
---

删除链表的倒数第N个元素。

<!-- more -->

---

### 19. Remove Nth Node From End of List

Given a linked list, remove the *n*-th node from the end of list and return its head.

**Example:**

```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

**Note:**

Given *n* will always be valid.

**Follow up:**

Could you do this in one pass?

---

对于链表的删除操作我们通常需要使用前面一个节点，将前一个节点的next指针指向要删除节点的next节点。

```java
dummy.next = head;
preNode = dummy;
//然后我们将preNode指针移动到要删除节点的前面
preNode.next = preNode.next.next;
return dummy.next;
```

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode fast = dummy;
    ListNode slow = dummy;
    for (int i = 0; i < n; i++) {
        fast = fast.next;
    }
    while (fast.next != null) {
        fast = fast.next;
        slow = slow.next;
    }
    slow.next = slow.next.next;
    return dummy.next;
}
```





