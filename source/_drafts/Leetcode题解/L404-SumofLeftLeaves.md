---
title: Leetcode404. Sum of Left Leaves
tags:
  - 二叉树
comments: true
date: 2018-04-02 22:56:43
categories: Leetcode题解
---
累加二叉树所有左叶子节点的值。

<!-- more -->

### 404. Sum of Left Leaves

Find the sum of all left leaves in a given binary tree.

**Example:**

```
    3
   / \
  9  20
    /  \
   15   7

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
```



算法：基于前序遍历算法，判断一个叶子节点是否是左叶子节点，然后累加。

```java
public int sum = 0;
public int sumOfLeftLeaves(TreeNode root) {
    if (root != null) preorderWalk(root);
    return sum;
}
public void preorderWalk(TreeNode root) {
    if (root == null) return;
    if (root.left != null) {
        if (root.left.right == null && root.left.left == null) sum += root.left.val;
    }
    preorderWalk(root.left);
    preorderWalk(root.right);
}
```

