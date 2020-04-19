---
title: Leetcode538. Convert BST to Greater Tree
tags:
  - 二叉树
comments: true
mathjax: false
date: 2018-04-06 19:13:08
categories: Leetcode题解
---

对二叉树的每个节点加上比其大的所有节点的值。

<!-- more -->

### 538. Convert BST to Greater Tree

Given a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.

**Example:**

```
Input: The root of a Binary Search Tree like this:
              5
            /   \
           2     13

Output: The root of a Greater Tree like this:
             18
            /   \
          20     13
```

#### 算法：后序遍历过程中叠加节点值

```java
private int sum = 0;
public TreeNode convertBST(TreeNode root) {
    convert(root);
    return root;
}
private void convert(TreeNode root) {
    if (root == null) return;
    convert(root.right);
    root.val += sum;
    sum = root.val;
    convert(root.left);
}
```

