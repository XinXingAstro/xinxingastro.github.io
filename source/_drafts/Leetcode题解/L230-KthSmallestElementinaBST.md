---
title: Leetcode230. Kth Smallest Element in a BST
tags:
  - 二叉树
comments: true
date: 2018-04-02 22:27:09
categories: Leetcode题解
---
在搜索二叉树中找到第K小的数。

<!-- more -->

### 230. Kth Smallest Element in a BST

Given a binary search tree, write a function `kthSmallest` to find the **k**th smallest element in it.

**Note:** 
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

**Follow up:**
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

**Credits:**
Special thanks to [@ts](https://leetcode.com/discuss/user/ts) for adding this problem and creating all test cases.



算法：由于搜索二叉树的中序遍历序列的从小到大排序的，所以只要找到中序遍历序列的第K个数即可：

```java
public int count = 0;
public int ans = 0;
public int kthSmallest(TreeNode root, int k) {
    count = k;
    if (root != null) inorderWalk(root);
    return ans;
}
public void inorderWalk(TreeNode root) {
    if (root == null) return;
    inorderWalk(root.left);
    count--;
    if (count == 0) {
        ans = root.val;
        return;
    }
    inorderWalk(root.right);
}
```

