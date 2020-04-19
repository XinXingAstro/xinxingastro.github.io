---
title: Leetcode235/236. Lowest Common Ancestor of a Binary (Search) Tree
tags:
  - 二叉树
  - 二叉搜索树
comments: true
date: 2018-04-01 18:07:30
categories: Leetcode题解
---
找一个二叉树或二叉搜索树的**最低公共祖先**。

<!-- more -->

### 235. Lowest Common Ancestor of a Binary Search Tree

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow **a node to be a descendant of itself**).”

```
        _______6______
       /              \
    ___2__          ___8__
   /      \        /      \
   0      _4       7       9
         /  \
         3   5
```

For example, the lowest common ancestor (LCA) of nodes `2` and `8` is `6`. Another example is LCA of nodes `2` and `4` is `2`, since a node can be a descendant of itself according to the LCA definition.



算法：由于是搜索二叉树，题目给出的两个节点的最低公共祖先节点的值，肯定比其中一个节点的值大，比另一个节点的值小。

- 如果当前节点的值比p和q的值都大，则向左遍历。
- 如果当前节点的值比p和q都小，则向右遍历。
- 直到找到一个节点的值介于p和q的值之间。

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    while ((root.val - p.val) * (root.val - q.val) > 0)
        root = p.val < root.val ? root.left : root.right;
    return root;
}
```





### 236. Lowest Common Ancestor of a Binary Tree

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow **a node to be a descendant of itself**).”

```
        _______3______
       /              \
    ___5__          ___1__
   /      \        /      \
   6      _2       0       8
         /  \
         7   4
```

For example, the lowest common ancestor (LCA) of nodes `5` and `1` is `3`. Another example is LCA of nodes `5` and `4` is `5`, since a node can be a descendant of itself according to the LCA definition.



算法：递归判断每一个节点的左子树或者右子树是否包含这两个节点。

- 如果一个节点的左右子树中没有两个节点的任何一个，则返回null。
- 如果一个节点的左子树为null，则返回右子树。
- 如果一个节点的右子树为null，则返回左子树。
- 如果一个节点左子树不为null，右子树也不为null，则这个节点就是以上两个节点的最低公共祖先。

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) return root;
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);
    if (left != null && right != null) return root;
    return left != null ? left : right;
}
```

