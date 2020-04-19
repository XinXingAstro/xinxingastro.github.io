---
title: Leetcode234. Palindrome Linked List
tags:
  - 链表
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-06 20:58:58
categories: Leetcode题解
---

判断一个链表是否是回文字。这道题是链表反转算法的应用。

<!-- more -->

---

### 234. Palindrome Linked List

Given a singly linked list, determine if it is a palindrome.

**Example 1:**

```
Input: 1->2
Output: false
```

**Example 2:**

```
Input: 1->2->2->1
Output: true
```

**Follow up:**
Could you do it in O(n) time and O(1) space?

---

#### 算法：反转后半部分链表然后对比

回文字具有以中心对称的特性，由于单链表只能从左到右遍历，所以我们不能使用中性扩散法，只能将后半部分链表翻转，然后对比前后两部分链表，如果各节点值相同，则是回文字。

算法分三步：

1）找到链表中点（如果节点个数是奇数，则后半部分多一个节点）

2）对后半部分链表进行反转

3）对比两部分节点的值，如果出现不同的值返回false，否则返回true

```java
public boolean isPalindrome(ListNode head) {
    if (head == null || head.next == null) return true;
    ListNode pre = null;
    ListNode slow = head;
    ListNode fast = head;
    // 找链表中点的算法也是使用三个指针，是固定写法。
    while (fast != null && fast.next != null) {
        pre = slow;
        slow = slow.next;
        fast = fast.next.next;
    }
    pre.next = null;
    pre = null;
    fast = slow.next;
    while (fast != null) {
        slow.next = pre;
        pre = slow;
        slow = fast;
        fast = fast.next;
    }
    slow.next = pre;
    pre = head;
    while (pre != null && slow != null) {
        if (pre.val != slow.val) return false;
        pre = pre.next;
        slow = slow.next;
    }
    return true;
}
```

时间复杂度：O(n)

空间复杂度：O(1)