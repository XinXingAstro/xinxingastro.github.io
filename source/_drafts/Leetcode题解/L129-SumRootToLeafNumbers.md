---
title: Leetcode129. Sum Root To Leaf Numbers
tags:
  - 二叉树
comments: true
date: 2018-03-30 23:33:13
categories: Leetcode题解
---
二叉树的每条从根节点到叶节点的路径都代表一个数字，累加所有数字。

<!-- more -->

### 129. Sum Root to Leaf Numbers

Given a binary tree containing digits from `0-9` only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path `1->2->3` which represents the number `123`.

Find the total sum of all root-to-leaf numbers.

For example,

```
    1
   / \
  2   3
```

The root-to-leaf path `1->2` represents the number `12`.
The root-to-leaf path `1->3` represents the number `13`.

Return the sum = 12 + 13 = `25`.



算法：基于二叉树前序遍历算法，累加所有路径代表的数字。

```java
public int sumNumbers(TreeNode root) {
    return getSum(root, 0);
}
public int getSum(TreeNode root, int sum) {
    if (root == null) return 0;
    sum = sum * 10 + root.val;
    if (root.left == null && root.right == null) return sum;
    return getSum(root.left, sum) + getSum(root.right, sum);
}
```

