---
title: Leetcode102. Binary Tree Level Order Traversal
tags:
  - 二叉树
comments: true
date: 2018-04-01 16:56:19
categories: Leetcode题解
---
二叉树的层次遍历的递归和迭代算法。

<!-- more -->

### 102. Binary Tree Level Order Traversal

Given a binary tree, return the *level order* traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its level order traversal as:

```
[
  [3],
  [9,20],
  [15,7]
]
```



**迭代算法：**

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> ans = new ArrayList<>();
    if (root == null) return ans;
    Queue<TreeNode> queue = new ArrayDeque<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        List<Integer> tmp = new ArrayList<>();
        for (int i = 0; i < size; i++) {
            TreeNode p = queue.poll();
            tmp.add(p.val);
            if (p.left != null) queue.offer(p.left);
            if (p.right != null) queue.offer(p.right);
        }
        ans.add(tmp);
    }
    return ans;
}
```



**迭代算法：**

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> ans = new ArrayList<>();
    if (root == null) return ans;
    levelOrderWalk(root, 0, ans);
    return ans;
}
public void levelOrderWalk(TreeNode root, int level, List<List<Integer>> ans) {
    if (root == null) return;
    if (level >= ans.size()) ans.add(new ArrayList<>());
    ans.get(level).add(root.val);
    levelOrderWalk(root.left, level+1, ans);
    levelOrderWalk(root.right, level+1, ans);
}
```

