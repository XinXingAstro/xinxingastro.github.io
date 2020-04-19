---
title: Leetcode99. Recover Binary Search Tree
tags:
  - 二叉树
  - 搜索二叉树
comments: true
date: 2018-03-29 16:04:09
categories: Leetcode题解
---
搜索二叉树中有一对节点被交换过，恢复该搜索二叉树，使用二叉树前序遍历算法。

<!-- more -->

### 99. Recover Binary Search Tree

Two elements of a binary search tree (BST) are swapped by mistake.

Recover the tree without changing its structure.

Note:

A solution using O(n) space is pretty straight forward. Could you devise a constant space solution?



算法：由于搜索二叉树的前序遍历序列是从小到大排序的，如果有一对节点的值被交换，这前序遍历序列中会出现两个逆序对，如下所示：

```
1 2 3 4 5 //交换1和5后
5 2 3 4 1
```

序列中出现 5 2 和 4 1 两个逆序对，我们定义两个变量，一个指向第一个逆序对的第一个数字，第二个变量指向第二个逆序对的后面一个数字，交换这两个数字就可以恢复搜索二叉树。

```java
public TreeNode preNode = null;
public TreeNode node1 = null;
public TreeNode node2 = null;

public void recoverTree(TreeNode root) {
    if (root == null) return;

    inorderWalk(root);

    int temp = node1.val;
    node1.val = node2.val;
    node2.val = temp;
}
public void inorderWalk(TreeNode root) {
    if (root == null) return;

    inorderWalk(root.left);

    if (node1 == null && preNode != null && preNode.val > root.val)
        node1 = preNode;
    if (node1 != null && preNode != null && preNode.val > root.val)
        node2 = root;
    preNode = root;

    inorderWalk(root.right);
}
```

