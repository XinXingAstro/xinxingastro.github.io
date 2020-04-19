---
title: Leetcode98. Validate Binary Search Tree
tags:
  - 二叉树
comments: true
date: 2018-04-01 09:53:41
categories: Leetcode题解
---
判断二叉树是否是搜索二叉树。

<!-- more -->

### 98. Validate Binary Search Tree

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example 1:**

```
    2
   / \
  1   3
```

Binary tree 

`[2,1,3]`, return true.

**Example 2:**

```
    1
   / \
  2   3
```

Binary tree 

`[1,2,3]`, return false.



由于搜索二叉树的中序遍历序列是从小到大排序的，所以可以利用中序遍历算法进行判断

算法1:

```java
ArrayList<Integer> list = new ArrayList<>();
public boolean isValidBST(TreeNode root) {
    inorderWalk(root);
    int size = list.size();
    for (int i = 1; i < size; i++) {
        if (list.get(i-1) >= list.get(i)) return false;
    }
    return true;
}
public void inorderWalk(TreeNode root) {
    if (root == null) return;
    inorderWalk(root.left);
    list.add(root.val);
    inorderWalk(root.right);
}
```



算法2:

```java
public boolean isValidBST02(TreeNode root) {
    //这里必须是Long类型，测试用例里有大数节点
    return helper(root, Long.MIN_VALUE, Long.MAX_VALUE);
}
public boolean helper(TreeNode root, long min, long max){
    if(root == null) return true;
    if(root.val >= max || root.val <= min) return false;
    return helper(root.left, min, root.val) && helper(root.right, root.val, max);
}
```