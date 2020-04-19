---
title: Leetcode222. Count Complete Tree Nodes
tags:
  - 二叉树
comments: true
date: 2018-03-31 17:54:52
categories: Leetcode题解
---
统计二叉树的节点总数。

<!-- more -->

### 222. Count Complete Tree Nodes

Given a **complete** binary tree, count the number of nodes.

**Definition of a complete binary tree from Wikipedia:**
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

算法：判断树是否是完全二叉树，如果是直接返回树的节点数，如果不是则递归统计树的节点数：

```java
public int countNodes(TreeNode root) {
    if (root == null) return 0;
    TreeNode left = root, right = root;
    int height = 0;
    while (right != null) {
        left = left.left;
        right = right.right;
        height++;
    }
    if (left == null) return (1 << height) - 1;
    return 1 + countNodes(root.left) + countNodes(root.right);
}
```