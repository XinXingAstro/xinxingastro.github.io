---
title: Leetcode654. Maximum Binary Tree
tags:
  - 二叉树
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-20 11:00:28
categories: Leetcode题解
---

给出一个数组，用该数组构建一个最大二叉树。

<!-- more -->

---

### 654. Maximum Binary Tree

Given an integer array with no duplicates. A maximum tree building on this array is defined as follow:

1. The root is the maximum number in the array. 
2. The left subtree is the maximum tree constructed from left part subarray divided by the maximum number.
3. The right subtree is the maximum tree constructed from right part subarray divided by the maximum number.

Construct the maximum tree by the given array and output the root node of this tree.

**Example 1:**

```
Input: [3,2,1,6,0,5]
Output: return the tree root node representing the following tree:

      6
    /   \
   3     5
    \    / 
     2  0   
       \
        1
```

**Note:**

1. The size of the given array will be in the range [1,1000].

---

#### 算法：递归建树

每次在数组指定范围中每次选出最大元素新建节点，递归建树，其中递归建树的过程是固定写法。

```java
public TreeNode constructMaximumBinaryTree(int[] nums) {
    if (nums == null || nums.length == 0)
        return null;
    return build(nums, 0, nums.length-1);
}
public TreeNode build(int[] nums, int start, int end) {
    if (start > end) 
        return null;
    int max = Integer.MIN_VALUE;
    int index = -1;
    for (int i = start; i <= end; i++) {
        if (nums[i] > max) {
            max = nums[i];
            index = i;
        }   
    }
    TreeNode root = new TreeNode(max);
    root.left = build(nums, start, index - 1);
    root.right = build(nums, index + 1, end);
    return root;
}
```

