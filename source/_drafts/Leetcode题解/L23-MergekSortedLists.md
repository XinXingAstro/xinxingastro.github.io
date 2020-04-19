---
title: Leetcode23. Merge k Sorted Lists
tags:
  - 链表
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-06 23:59:03
categories: Leetcode题解
---

合并k个排序的链表。

这道题完美的体现了分治法的运用对算法时间效能的提升效果。

<!-- more -->

---

### 23. Merge k Sorted Lists

Merge *k* sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

**Example:**

```
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```

---

#### 算法1: 一个一个合并

首先想到的算法就是从上到下两个两个合并，运用两个排序链表的合并算法，我们最终可以合并所有链表。我将两个排序链表的合并操作单独写成一个方法，在for循环中连续调用这个方法，就可以合并所有的链表。

```java
ListNode dummy = new ListNode(-1);
public ListNode mergeKLists(ListNode[] lists) {
    if (lists == null) return null;
    if (lists.length == 1) return lists[0];
    ListNode ans = null;
    for (int i = 0; i < lists.length; i++) {
        if (lists[i] != null)
            ans = merge(ans, lists[i]);
    }
    return ans;
}
public ListNode merge(ListNode l1, ListNode l2) {
    ListNode l = dummy;
    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            l.next = l1;
            l1 = l1.next;
        } else {
            l.next = l2;
            l2 = l2.next;
        }
        l = l.next;
    }
    if (l1 != null) l.next = l1;
    if (l2 != null) l.next = l2;
    return dummy.next;
}
```

时间复杂度：O(n^2)

空间复杂度：O(1)

Leetcode运行时间：206ms

#### 算法2: 模拟归并排序算法

相较于上面简单粗暴的逐个合并链表，我们使用分治法的思想对算法进行优化。先合并左半部分链表，再合并右半部分链表，然后把两部分链表合并起来就得到了最终解。

可以看到合并算法和上面用的是一模一样的，我们只是运用分治法的思想对算法做了一点改动，算法的运行时间就缩短到了12ms。

```java
public ListNode mergeKLists(ListNode[] lists) {
    if (lists == null) return null;
    return merge(0, lists.length - 1, lists);
}
public ListNode merge(int start, int end, ListNode[] lists) {
    if (start > end) return null;
    if (start == end) return lists[start];
    int mid = (start + end) >> 1;
    ListNode l = merge(start, mid, lists);
    ListNode r = merge(mid+1, end, lists);

    ListNode dummy = new ListNode(-1);
    ListNode cur = dummy;
    while (l != null && r != null) {
        if (l.val < r.val) {
            cur.next = l;
            l = l.next;
        } else {
            cur.next = r;
            r = r.next;
        }
        cur = cur.next;
    }
    if (l != null) cur.next = l;
    if (r != null) cur.next = r;
    return dummy.next;
}
```

时间复杂度：O(nlogn)

空间复杂度：O(1)

Leetcode运行时间: 12ms