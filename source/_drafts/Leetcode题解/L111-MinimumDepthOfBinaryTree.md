---
title: Leetcode111. Minimum Depth Of Binary Tree
tags:
  - 二叉树
comments: true
date: 2018-03-29 18:57:21
categories: Leetcode题解
---
找到二叉树的最小深度，基于二叉树深度优先遍历算法。

<!-- more -->

### 111. Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.



```java
public int minDepth = Integer.MAX_VALUE;
public int minDepth(TreeNode root) {
    if (root == null) return 0;
    preorderWalk(root, 1);
    return minDepth;
}
public void preorderWalk(TreeNode root, int level) {
    if (root == null) return;
    preorderWalk(root.left,level+1);
    preorderWalk(root.right, level+1);
    if (root.left == null && root.right == null && level < minDepth) minDepth = level;
}
```

