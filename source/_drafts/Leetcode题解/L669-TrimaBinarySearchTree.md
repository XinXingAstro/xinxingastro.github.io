---
title: Leetcode669. Trim a Binary Search Tree
tags:
  - 二叉树
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-29 19:05:52
categories: Leetcode题解
---

对搜索二叉树进行剪枝，让二叉树中的每个节点都落在L～R的区间内。

<!-- more -->

---

### 669. Trim a Binary Search Tree

Given a binary search tree and the lowest and highest boundaries as `L` and `R`, trim the tree so that all its elements lies in `[L, R]` (R >= L). You might need to change the root of the tree, so the result should return the new root of the trimmed binary search tree.

**Example 1:**

```
Input: 
    1
   / \
  0   2

  L = 1
  R = 2

Output: 
    1
      \
       2
```

**Example 2:**

```
Input: 
    3
   / \
  0   4
   \
    2
   /
  1

  L = 1
  R = 3

Output: 
      3
     / 
   2   
  /
 1
```

---

#### 算法：先跟遍历剪枝

对搜索二叉树进行先跟遍历剪枝。

```java
public TreeNode trimBST(TreeNode root, int L, int R) {
    if (root == null) return null;
    if (root.val > R) return trimBST(root.left, L, R);
    if (root.val < L) return trimBST(root.right, L, R);

    root.left = trimBST(root.left, L, R);
    root.right = trimBST(root.right, L, R);
    return root;
}
```

时间复杂度：O(n)

空间复杂度：O(n)