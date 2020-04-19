---
title: Leetcode116/117. Populating Next Right Pointers in Each Node I/II
tags:
  - 二叉树
comments: true
date: 2018-03-30 19:15:32
categories: Leetcode题解
---
将每个节点右next指针指向同层右边的节点。

<!-- more -->

### 116. Populating Next Right Pointers in Each Node

Given a binary tree

```
    struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

**Note:**

- You may only use constant extra space.
- You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).

For example,
Given the following perfect binary tree,

```
         1
       /  \
      2    3
     / \  / \
    4  5  6  7
```

After calling your function, the tree should look like:

```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \  / \
    4->5->6->7 -> NULL
```



算法：从二叉树的左侧从上到下遍历，然后连接每一层的节点。

```java
public void connect(TreeLinkNode root) {
    while (root != null && root.left != null) {
        TreeLinkNode pNode = root;
        while (pNode != null) {
            pNode.left.next = pNode.right;
            pNode.right.next = pNode.next == null ? null : pNode.next.left;
            pNode = pNode.next;
        }
        root = root.left;
    }
}
```





### 117. Populating Next Right Pointers in Each Node II

Follow up for problem "*Populating Next Right Pointers in Each Node*".

What if the given tree could be any binary tree? Would your previous solution still work?

**Note:**

- You may only use constant extra space.

For example,
Given the following binary tree,

```
         1
       /  \
      2    3
     / \    \
    4   5    7
```

After calling your function, the tree should look like:

```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \    \
    4-> 5 -> 7 -> NULL
```



算法有两种，最先想到的是基于层次遍历，遍历每层的时候修改该层里的节点指针，但是层次遍历需要用到队列，需要O(n)的空间复杂度，题目要求使用O(1)的空间复杂度，所以需要使用临时变量的方法。想象在遍历二叉树每层时在该层左边定义一个新的节点，该节点的next指针指向二叉树该层的开始节点，然后依次向下遍历：

Leetcode用时1ms：

```java
public void connect(TreeLinkNode root) {
    while (root != null) {
        TreeLinkNode preNode = new TreeLinkNode(0);
        TreeLinkNode p = preNode;
        while (root != null) {
            if (root.left != null) {
                p.next = root.left;
                p = p.next;
            }
            if (root.right != null) {
                p.next = root.right;
                p = p.next;
            }
            root = root.next;
        }
        root = preNode.next;
    }
}
```



基于层次遍历的算法，Leetcode用时4ms，但是更好理解：

```java
public static void connect(TreeLinkNode root) {
    if (root == null) return;
    ArrayDeque<TreeLinkNode> queue = new ArrayDeque<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            TreeLinkNode pNode = queue.poll();
            if (i != (size-1)) pNode.next = queue.peek();
            if (pNode.left != null) queue.offer(pNode.left);
            if (pNode.right != null) queue.offer(pNode.right);
        }
    }
}
```

