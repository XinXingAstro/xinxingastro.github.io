---
title: Leetcode108. Convert Sorted Array to Binary Search Tree
tags:
  - 二叉树
comments: true
date: 2018-04-02 16:17:41
categories: Leetcode题解
---
将有序数组转化为**平衡的搜索二叉树**。

<!-- more -->

### 108. Convert Sorted Array to Binary Search Tree

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of *every* node never differ by more than 1.

**Example:**

```
Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
```



算法：由于题目要求转化为平衡的搜索二叉树，所以每次选取区间中间的元素新建节点，可以保证新建出的搜索二叉树的平衡的。

```java
public TreeNode sortedArrayToBST(int[] nums) {
    if (nums.length == 0 || nums == null) return null;
    return buildBST(nums, 0, nums.length-1);
}
public TreeNode buildBST(int[] nodes, int start, int end) {
    if (start > end) return null;
    int mid = (start + end) >> 1;
    TreeNode root = new TreeNode(nodes[mid]);
    root.left = buildBST(nodes, start, mid-1);
    root.right = buildBST(nodes, mid+1, end);
    return root;
}
```

