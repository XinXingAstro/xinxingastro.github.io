---
title: Leetcode572. Subtree of Another Tree
tags:
  - 二叉树
comments: true
mathjax: false
date: 2018-04-11 20:22:17
categories: Leetcode题解
---

判断一个二叉树是不是另一个二叉树的子树。

<!-- more -->

### 572. Subtree of Another Tree

Given two non-empty binary trees **s** and **t**, check whether tree **t** has exactly the same structure and node values with a subtree of **s**. A subtree of **s** is a tree consists of a node in **s** and all of this node's descendants. The tree **s** could also be considered as a subtree of itself.

**Example 1:**
Given tree s:

```
     3
    / \
   4   5
  / \
 1   2
```

Given tree t:

```
   4 
  / \
 1   2
```

Return **true**

because t has the same structure and node values with a subtree of s.

**Example 2:**
Given tree s:

```
     3
    / \
   4   5
  / \
 1   2
    /
   0
```

Given tree t:

```
   4
  / \
 1   2
```

Return **false**.



#### 算法1: 对比两二叉树遍历序列

分别对两个二叉树进行前序遍历（或者后序遍历或者中序遍历），如果t遍历出来的字符串是s遍历字符串的子串，则t是s的子树，否则不是子树。

注意：1. 生成字符串时，要在节点前加分隔符。2. 左右null节点要有区别，比如左边null表示为lnull，右边null表示为rnull。



#### 算法2: 递归遍历二叉树判断各节点是否相等

利用二叉树的遍历算法遍历s，一边遍历一边检查当前节点的值是否等于t树根节点的值，如果相等，则设置标志位为true，然后递归遍历两个数的各节点，如果所有节点值都相等，则返回true，如果出现不等的值，则返回false。

```java
public boolean isSubtree(TreeNode s, TreeNode t) {
    return isSame(s, t, false);
}
public boolean isSame(TreeNode s, TreeNode t, boolean flag) {
    if (s == null && t == null) return true;
    if (s == null || t == null) return false;
    if (flag && s.val != t.val) return false; //之前节点相等当前节点不等
    if (s.val == t.val && isSame(s.left, t.left, true) && isSame(s.right, t.right, true)) return true;
    else return isSame(s.left, t, false) || isSame(s.right, t, false);
}
```

