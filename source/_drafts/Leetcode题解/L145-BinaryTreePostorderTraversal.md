---
title: Leetcode145. Binary Tree Postorder Traversal
tags:
  - 二叉树
comments: true
date: 2018-03-31 00:21:01
categories: Leetcode题解
---
输出二叉树的后序遍历序列。

<!-- more -->

### 145. Binary Tree Postorder Traversal

Given a binary tree, return the *postorder* traversal of its nodes' values.

For example:
Given binary tree `[1,null,2,3]`,

```
   1
    \
     2
    /
   3
```

return `[3,2,1]`.

**Note:** Recursive solution is trivial, could you do it iteratively?



```java
public List<Integer> postorderTraversal(TreeNode root) {
    ArrayDeque<TreeNode> stack = new ArrayDeque<>();
    LinkedList<Integer> res = new LinkedList<>();
    TreeNode p = root;
    while (p != null || !stack.isEmpty()) {
        while (p != null) {
            res.addFirst(p.val);
            stack.push(p);
            p = p.right;
        }
        if (!stack.isEmpty()) {
            p = stack.pop().left;
        }
    }
    return res;
}
```