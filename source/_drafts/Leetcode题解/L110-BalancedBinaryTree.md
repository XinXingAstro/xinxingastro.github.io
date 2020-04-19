---
title: Leetcode110. Balanced Binary Tree
tags:
  - 二叉树
comments: true
date: 2018-03-29 17:57:48
categories: Leetcode题解
---
判断一个二叉树是否是平衡二叉树。

<!-- more -->

### 110. Balanced Binary Tree

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

> a binary tree in which the depth of the two subtrees of *every* node never differ by more than 1.

**Example 1:**

Given the following tree `[3,9,20,null,null,15,7]`:

```
    3
   / \
  9  20
    /  \
   15   7
```

Return true.
**Example 2:**

Given the following tree `[1,2,2,3,3,null,null,4,4]`:

```
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
```

Return false.



算法1: 自顶向下递归判断每一个节点的左右子树的深度差是否小于1，时间复杂度O(n^2)​，Leetcode运行时间：3ms

```java
public int depth(TreeNode root) {
    if (root == null) return 0;
    return Math.max(depth(root.left), depth(root.right)) + 1;
}
public boolean isBalanced(TreeNode root) {
    if (root == null) return true;
    int left = depth(root.left);
    int right = depth(root.right);
    return Math.abs(left - right) <= 1 && isBalanced(root.left) && isBalanced(root.right);
}
```



算法2: 自底向上依次判断每个节点是否是平衡的，基于二叉树的后序遍历算法实现，时间复杂度O(n)，Leetcode运行时间：2ms

```java
public int depth(TreeNode root) {
    if (root == null) return 0;

    int leftHeight = depth(root.left);
    if (leftHeight == -1) return -1;
    int rightHeight = depth(root.right);
    if (rightHeight == -1) return -1;

    if (Math.abs(leftHeight - rightHeight) > 1) return -1;
    return Math.max(leftHeight, rightHeight) + 1;
}
public boolean isBalanced(TreeNode root) {
    return depth(root) != -1;
}
```

