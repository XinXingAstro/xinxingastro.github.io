---
title: Leetcode104. Maximum Depth of Binary Tree
tags:
  - 二叉树
comments: true
date: 2018-04-02 14:11:24
categories: Leetcode题解
---
找到二叉树的最大深度

<!-- more -->

### 104. Maximum Depth of Binary Tree

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

For example:
Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its depth = 3.



算法：根据深度优先遍历查找二叉树最大深度：

```java
public int maxDep = 0;
public int maxDepth(TreeNode root) {
    if (root != null) preorderWalk(root, 1);
    return maxDep;
}
public void preorderWalk(TreeNode root, int level) {
    if (root == null) return;
    if (root.left == null && root.right == null) if (level > maxDep) maxDep = level;
    preorderWalk(root.left, level+1);
    preorderWalk(root.right, level+1);
}
```

