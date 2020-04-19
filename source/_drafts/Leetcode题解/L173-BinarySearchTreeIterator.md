---
title: Leetcode173. Binary Search Tree Iterator
tags:
  - 二叉树
  - 搜索二叉树
comments: true
date: 2018-03-31 15:39:09
categories: Leetcode题解
---
前序遍历搜索二叉树。

<!-- more -->

### 173. Binary Search Tree Iterator

Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling `next()` will return the next smallest number in the BST.

**Note:** `next()` and `hasNext()` should run in average O(1) time and uses O(*h*) memory, where *h* is the height of the tree. 

**Credits:**
Special thanks to [@ts](https://oj.leetcode.com/discuss/user/ts) for adding this problem and creating all test cases.



```java
public class BSTIterator {
    ArrayDeque<TreeNode> stack = new ArrayDeque<>();

    public BSTIterator(TreeNode root) {
        addAll(root);
    }

    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return !stack.isEmpty();
    }

    /** @return the next smallest number */
    public int next() {
        TreeNode p = stack.pop();
        addAll(p.right);
        return p.val;
    }

    public void addAll(TreeNode root) {
        while (root != null) {
            stack.push(root);
            root = root.left;
        }
    }
}
```

