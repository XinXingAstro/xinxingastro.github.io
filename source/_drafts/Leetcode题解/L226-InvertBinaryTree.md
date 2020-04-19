---
title: Leetcode226. Invert Binary Tree
tags:
  - 二叉树
comments: true
date: 2018-03-31 18:14:11
categories: Leetcode题解
---
二叉树的镜像。

<!-- more -->

### 226. Invert Binary Tree

Invert a binary tree.

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

to

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

**Trivia:**
This problem was inspired by [this original tweet](https://twitter.com/mxcl/status/608682016205344768) by [Max Howell](https://twitter.com/mxcl):

> Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so f*** off.



算法：基于二叉树的层次遍历算法，翻转每个节点的左右子树

算法1迭代算法，Leetcode 1ms：

```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) return null;
    ArrayDeque<TreeNode> queue = new ArrayDeque<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            TreeNode p = queue.poll();
            if (p.left != null) queue.offer(p.left);
            if (p.right != null) queue.offer(p.right);
            TreeNode tmp = p.left;
            p.left = p.right;
            p.right = tmp;
        }
    }
    return root;
}
```



算法2递归算法，Leetcode 0ms：

```java
public TreeNode invertTree(TreeNode root) {
    if(root == null || (root.left == null && root.right == null)) return root;
    TreeNode tmp = root.left;
    root.left = invertTree(root.right);
    root.right = invertTree(tmp);
    return root;
}
```

