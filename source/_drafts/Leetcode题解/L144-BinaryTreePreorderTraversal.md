---
title: Leetcode144. Binary Tree Preorder Traversal
tags:
  - 二叉树
comments: true
date: 2018-03-30 23:52:24
categories: Leetcode题解
---
输出二叉树的前序遍历序列。

<!-- more -->

### 144. Binary Tree Preorder Traversal

Given a binary tree, return the *preorder* traversal of its nodes' values.

For example:
Given binary tree `[1,null,2,3]`,

```
   1
    \
     2
    /
   3
```

return `[1,2,3]`.

**Note:** Recursive solution is trivial, could you do it iteratively?

递归算法：

```java
List<Integer> list = new ArrayList<>();
public List<Integer> preorderTraversal(TreeNode root) {
    if (root == null) return new ArrayList<>();
    preorderWalk(root);
    return list;
}
public void preorderWalk(TreeNode root) {
    if (root == null) return;
    list.add(root.val);
    preorderWalk(root.left);
    preorderWalk(root.right);
}
```

迭代算法：

```java
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<>();
    ArrayDeque<TreeNode> stack = new ArrayDeque<>();
    TreeNode p = root;
    while (p != null || !stack.isEmpty()) {
        while (p != null) {
            list.add(p.val);
            stack.push(p);
            p = p.left;
        }
        if (!stack.isEmpty()) {
            p = stack.pop();
            p = p.right;
        }
    }
    return list;
}
```