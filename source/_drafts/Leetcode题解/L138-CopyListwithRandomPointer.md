---
title: Leetcode138. Copy List with Random Pointer
tags:
  - 链表
  - HashMap
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-06 00:13:10
categories: Leetcode题解
---

复制节点中带有随机指针的链表。

这道题就是剑指Offer面试题35: 复杂链表的复制。

<!-- more -->

---

### 138. Copy List with Random Pointer

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

```java
Definition for singly-linked list with a random pointer.
class RandomListNode {
    int label;
    RandomListNode next, random;
    RandomListNode(int x) { this.label = x; }
}
```

---

这道题的难点在于复制random指针，random指向链表中某一个节点，要复制这个链表，我们必须要通过这个节点找到对应的复制节点。直觉：建立原节点和复制节点的一一对应关系，我们通过原节点就可以找到复制节点。我们有两种算法。

#### 算法1: 使用HashMap存储Node和CopyNode

由于节点中带有一个随机指针，为了在复制的时候能知道这个随机指针指向的到底是哪个节点，我们必须需要一种存储结构将原节点和复制出的节点一一对应，换句话说就是能用原节点找到对应的复制节点。

基本思想是使用HashMap，以原节点作为key，复制节点作为value，我们遍历两遍链表，第一遍先复制所有节点，然后设置所有节点的next指针，并且建立key-value关系，第二遍我们通过key-value值设置每个节点的random指针。

```java
public RandomListNode copyRandomList(RandomListNode head) {
    RandomListNode dummy = new RandomListNode(-1);
    RandomListNode l2 = dummy;
    RandomListNode l1 = head;
    HashMap<RandomListNode, RandomListNode> map = new HashMap<>();
    while (l1 != null) {
        l2.next = new RandomListNode(l1.label);
        l2 = l2.next;
        map.put(l1, l2);
        l1 = l1.next;
    }
    l2 = dummy.next;
    l1 = head;
    while (l1 != null) {
        l2.random = map.get(l1.random);
        l1 = l1.next;
        l2 = l2.next;
    }
    return dummy.next;
}
```

时间复杂度：O(n)

空间复杂度：O(n)

#### 算法2: 将复制节点链接到原节点的前面

对于链表内的每个节点，我们将复制出来的节点插入到原节点的前面，这样原节点的random指针指向节点的下一个节点，就是相应复制节点random指针应该指向的节点。

算法分三步：

1）将复制节点链接到链表中

![L13801](/images/L13801.png)

2）复制原链表各节点的random指针

![L13802](/images/L13802.png)

3）将原链表和复制链表拆开

![L13803](/images/L13803.png)

```java
public RandomListNode copyRandomList(RandomListNode head) {
    if (head == null) return null;
    RandomListNode l1 = head;
    RandomListNode l2;
    while (l1 != null) {
        l2 = new RandomListNode(l1.label);
        l2.next = l1.next;
        l1.next = l2;
        l1 = l2.next;
    }
    l1 = head;
    while (l1 != null) {
        l2 = l1.next;
        if (l1.random != null) {
            l2.random = l1.random.next;
        }
        l1 = l2.next;
    }
    l1 = head;
    l2 = l1.next;
    RandomListNode head2 = l2;
    while (l1.next != null) {
        l1.next = l2.next;
        if (l1.next == null) break;
        l1 = l1.next;
        l2.next = l1.next;
        l2 = l2.next;
    }
    return head2;
}
```

时间复杂度：O(n)

空间复杂度：O(1)