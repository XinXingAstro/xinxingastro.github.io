---
title: Leetcode101. Symmetric Tree
tags:
  - 二叉树
comments: true
date: 2018-04-01 16:11:36
categories: Leetcode题解
---
查看一个二叉树是否对称。

<!-- more -->

### 101. Symmetric Tree

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree `[1,2,2,3,4,4,3]` is symmetric:

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

But the following `[1,2,2,null,3,null,3]` is not:

```
    1
   / \
  2   2
   \   \
   3    3
```

**Note:**
Bonus points if you could solve it both recursively and iteratively.



算法1: 用二叉树层次遍历算法遍历每一层的节点，存在双端队列里，然后检查头尾节点，看该队列是否对称，Leetcode 21ms。

```java
public boolean isSymmetric(TreeNode root) {
    if (root == null) return true;
    LinkedList<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            TreeNode p = queue.poll();
            if (p == null) continue;
            queue.offer(p.left);
            queue.offer(p.right);
        }
        LinkedList<TreeNode> tmp = new LinkedList<>(queue);
        while (!tmp.isEmpty()) {
            TreeNode first = tmp.pollFirst();
            TreeNode last = tmp.pollLast();
            if (first == null || last == null) {
                if (first != null) return false;
                if (last != null) return false;
                continue;
            }
            if (first.val != last.val) return false;
        }
    }
    return true;
}
```

算法2: 判断每一个节点的左右子节点是否对称，然后递归判断，Leetcode 15ms：

```java
public boolean isSymmetric02(TreeNode root) {
    if (root == null) return true;
    return helper(root.left, root.right);
}
public boolean helper(TreeNode left, TreeNode right) {
    if (left == null) return right == null;
    if (right == null) return false;
    return left.val == right.val && helper(left.left, right.right) && helper(left.right, right.left);
}
```

