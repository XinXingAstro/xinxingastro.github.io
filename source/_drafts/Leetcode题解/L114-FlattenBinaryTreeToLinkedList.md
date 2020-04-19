---
title: Leetcode114. Flatten Binary Tree To Linked List
tags:
  - 二叉树
comments: true
date: 2018-03-30 18:26:09
categories: Leetcode题解
---
将二叉树转化为链表。

<!-- more -->

### 114. Flatten Binary Tree to Linked List

Given a binary tree, flatten it to a linked list in-place.

For example,
Given

```
         1
        / \
       2   5
      / \   \
     3   4   6
```

The flattened tree should look like:

```
   1
    \
     2
      \
       3
        \
         4
          \
           5
            \
             6
```

**Hints:**

If you notice carefully in the flattened tree, each node's right child points to the next node of a pre-order traversal.



算法：利用二叉树前序遍历算法的递归实现求解

```java
public TreeNode preNode = null;
public void flatten(TreeNode root) {
    if (root == null) return;
    if (preNode != null) {
        preNode.left = null;
        preNode.right = root;
    }
    preNode = root;
    //由于对左子节点递归访问时，所有节点的右子节点都被修改为当前节点，
    //所以要将当前节点的右子节点存储下来，对右子节点递归时用
    TreeNode right = root.right;
    flatten(root.left);
    flatten(right);
}
```

