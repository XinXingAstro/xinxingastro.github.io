---
title: Leetcode160. Intersection of Two Linked Lists
tags:
  - 链表
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-06 16:01:22
categories: Leetcode题解
---

找到两个链表的第一个公共节点。

剑指Offer面试题52：两个链表的第一个公共节点

<!-- more -->

---

### 160. Intersection of Two Linked Lists

Write a program to find the node at which the intersection of two singly linked lists begins.

 

For example, the following two linked lists: 

```
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗            
B:     b1 → b2 → b3
```

begin to intersect at node c1.

 

**Notes:**

- If the two linked lists have no intersection at all, return `null`.
- The linked lists must retain their original structure after the function returns. 
- You may assume there are no cycles anywhere in the entire linked structure.
- Your code should preferably run in O(n) time and use only O(1) memory.

**Credits:**
Special thanks to [@stellari](https://oj.leetcode.com/discuss/user/stellari) for adding this problem and creating all test cases.

---

#### 算法：得到两链表长度差后从相同起点开始遍历

算法分为一下几步：

1）我们首先计算出两个链表的长度差dif。

2）然后在长的那个链表上让指针从头开始走dif步。

3）然后将短链表的指针放到头部，两个指针同时向前走，由于两个指针相对于第一个公共节点的距离相同，如果存在公共节点，则两个指针肯定会相遇，第一次相遇的那个节点就是第一个公共节点。

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if (headA == null || headB == null) return null;
    ListNode l1 = headA;
    ListNode l2 = headB;
    int dif1 = 0;
    int dif2 = 0;
    while (l1 != null || l2 != null) {
        if (l1 != null) l1 = l1.next;
        else dif1++;
        if (l2 != null) l2 = l2.next;
        else dif2++;
    }
    l1 = headA;
    l2 = headB;
    if (dif1 != 0){
        for (int i = 0; i < dif1; i++) l2 = l2.next;
    }
    if (dif2 != 0) {
        for (int i = 0; i < dif2; i++) l1 = l1.next; 
    }
    while (l1 != null && l2 != null) {
        if (l1 == l2) return l1;
        l1 = l1.next;
        l2 = l2.next;
    }
    return null;
}
```

时间复杂度：O(n)

空间复杂度：O(1)

找出两链表第一个公共节点是一个非常基础的算法，这个算法可以用来找出二叉树中两节点的最低公共祖先：如果二叉树中每个节点都有一个指向父节点的指针，那么每条从叶节点到根节点的路径，就可以看成一个链表，两节点到根节点路径上的第一个交点，就是这两个节点的最低公共祖先。