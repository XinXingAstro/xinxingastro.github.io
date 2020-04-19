---
title: Leetcode142. Linked List Cycle II
tags:
  - 链表
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-06 21:55:14
categories: Leetcode题解
---

找到有环链表的入口节点。同样是很基础的算法。

剑指Offer面试题23: 链表中环的入口节点。

<!-- more -->

---

### 142. Linked List Cycle II

Given a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

**Note:** Do not modify the linked list.

**Follow up**:
Can you solve it without using extra space?

---

找到有环链表中的入口节点的算法是一个非常基础的算法，这个算法可以解决在一定条件下查找数组中的重复数字，见[面试题3：数组中重复的数字](https://xinxingastro.github.io/2018/05/03/剑指Offer/面试题3-数组中重复的数字/)，找出有环链表中的入口节点有两种算法。

#### 算法1: 龟兔赛跑法

首先来看一个结论。一个有环链表如下图所示，其中a点是链表头，b点是链表环入口，如果两个人X和Y同时从a点出发，X以一倍速度前进，Y以2倍速度前进，他们相遇的地点是c。

![L14201](/images/L14201.png)

假设Y在环中转了n圈后才遇到X，那么有：

2(A + B) = A + B + n(B + C) => A = C + (n-1)(B + C)

既a点到b点的距离 = (n-1)圆的周长 + c点到b点的距离，也就是说，如果X和Y在c点相遇后，X回到a点，然后X和Y以同样的速度继续前进，则他们最终会在b点也就是环的入口点相遇。这就是龟兔赛跑算法的原理。

算法分两步：

1）设置两个指针slow fast，slow以1倍速度前进，fast以2倍速度前进，直到他们相遇，如果fast到到达链表终点，则链表中不存在环。

2）slow回到head，fast在相遇点，两个指针同时以1倍速度前进，他们相遇的地点就是链表环的入口节点。

```java
public ListNode detectCycle(ListNode head) {
    if (head == null || head.next == null) return null;
    ListNode slow = head;
    ListNode fast = head;
    // 使用快慢指针时，这个循环条件是固定写法。
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) break;
    }
    if (fast == null || fast.next == null) return null;
    slow = head;
    while (slow != fast) {
        slow = slow.next;
        fast = fast.next;
    }
    return slow;
}
```

#### 算法2: 计算环内节点数

另一种方法，我们也是使用快慢指针，在两个指针相遇时，一个指针不动，另一个指针在环中转一圈就可以得到环中节点数。

算法分三步：

1）设置两个指针slow fast，slow以1倍速度前进，fast以2倍速度前进，直到他们相遇，如果fast到到达链表终点，则链表中不存在环。

2）fast不动，slow继续以一倍速度前进，统计记录前进步数，知道slow fast再次相遇，前进的步数就是环中的节点数n。

3）fast和slow回到head，fast前进n步，fast slow同时以1倍速度前进，两指针相遇的节点就是环的入口节点。