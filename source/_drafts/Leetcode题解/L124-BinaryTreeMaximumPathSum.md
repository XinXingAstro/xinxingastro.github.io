---
title: Leetcode124. Binary Tree Maximum Path Sum
tags:
  - 二叉树
comments: true
date: 2018-03-30 21:45:30
categories: Leetcode题解
---
计算出树中所有具有公共祖先节点的路径之中，节点值的和最大的路径。

<!-- more -->

### 124. Binary Tree Maximum Path Sum

Given a binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain **at least one node** and does not need to go through the root.

For example:
Given the below binary tree,

```
       1
      / \
     2   3
```

Return `6`.

题目的意思可以理解为，找出所有具有公共祖先节点的路径，在这些路径中找到节点值的和最大的那个路径。

算法：自底向上，利用后序遍历，计算每一个节点的路径和(以该节点为公共祖先的路径和)，然后找出和最大的节点。

```java
public int maxSum = Integer.MIN_VALUE;
public int maxPathSum(TreeNode root) {
    getSum(root);
    return maxSum;
}
public int getSum(TreeNode root) {
    if (root == null) return 0;
    int left = Math.max(0, getSum(root.left));
    int right = Math.max(0, getSum(root.right));
    maxSum = Math.max(maxSum, left+right+root.val);
    return Math.max(right, left) + root.val;
}
```

