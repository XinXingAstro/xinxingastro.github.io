---
title: Leetcode543. Diameter of Binary Tree
tags:
  - 二叉树
comments: true
mathjax: false
date: 2018-04-06 19:48:16
categories: Leetcode题解
---

计算二叉树的直径。二叉树的直径：二叉树中任意两个节点之间最长的路径。

<!-- more -->

### 543. Diameter of Binary Tree

Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the **longest** path between any two nodes in a tree. This path may or may not pass through the root.

**Example:**
Given a binary tree 

```
          1
         / \
        2   3
       / \     
      4   5    
```

Return **3**, which is the length of the path [4,2,1,3] or [5,2,1,3].

**Note:** The length of path between two nodes is represented by the number of edges between them.



#### 算法：递归遍历

计算左右子树最长路径，同时找出一个左右子树路径之和最大的节点。

```java
private int ans = 0;
public int diameterOfBinaryTree(TreeNode root) {
    getDiameter(root);
    return ans;
}
private int getDiameter(TreeNode root) {
    if (root == null) return 0;
    int l = getDiameter(root.left);
    int r = getDiameter(root.right);
    ans = Math.max(ans, l + r);
    return Math.max(l, r) + 1;
}
```



