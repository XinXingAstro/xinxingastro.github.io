---
title: Leetcode513. Find Bottom Left Tree Value
tags:
  - 二叉树
comments: true
date: 2018-04-03 23:36:11
categories: Leetcode题解
---
找二叉树最下面一层，最左边的元素。

<!-- more -->

### 513. Find Bottom Left Tree Value

Given a binary tree, find the leftmost value in the last row of the tree. 

**Example 1:**

```
Input:

    2
   / \
  1   3

Output:
1
```

**Example 2:** 

```
Input:

        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7

Output:
7
```

**Note:** You may assume the tree (i.e., the given root node) is not **NULL**.



算法1: 利用二叉树层次遍历算法，获得最下一层最左边元素。Leetcode 11ms

```java
//算法1：利用层次遍历找左下角元素
public int findBottomLeftValue(TreeNode root) {
    // if (root == null) return null;
    ArrayDeque<TreeNode> queue = new ArrayDeque<>();
    queue.offer(root);
    TreeNode p = root;
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            p = queue.poll();
            if (p.right != null) queue.offer(p.right);
            if (p.left != null) queue.offer(p.left);
        }
    }
    return p.val;
}
```



算法2: 利用二叉树前序遍历算法，没进入新的一层获取第一个遍历的元素。Leetcode 6ms

```java
//算法2：利用前序遍历找每次进入新层以后记录第一个元素
private int newLevel;
private int ans;
public int findBottomLeftValue01(TreeNode root) {
    preorderWalk(root, 0);
    return ans;
}
public void preorderWalk(TreeNode root, int level) {
    if (root == null) return;
    if (newLevel == level) {
        newLevel++;
        ans = root.val;
    }
    preorderWalk(root.left, level+1);
    preorderWalk(root.right, level+1);
}
```

