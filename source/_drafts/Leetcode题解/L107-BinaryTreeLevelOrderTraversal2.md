---
title: Leetcode107. Binary Tree Level Order Traversal II
tags:
  - 二叉树
comments: true
date: 2018-03-29 17:11:23
categories: Leetcode题解
---
从下向上层次遍历二叉树，二叉树层次遍历加上层次控制。

<!-- more -->

### 107. Binary Tree Level Order Traversal II

Given a binary tree, return the *bottom-up level order* traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:
Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its bottom-up level order traversal as:

```
[
  [15,7],
  [9,20],
  [3]
]
```



```java
public List<List<Integer>> levelOrderBottom(TreeNode root) {
        if (root == null) return new ArrayList<>();
        ArrayDeque<TreeNode> queue = new ArrayDeque<>();
        List<List<Integer>> ans = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            LinkedList<Integer> level = new LinkedList<>();
            for (int i = 0; i < size; i++) {
                TreeNode pNode = queue.poll();
                if (pNode.left != null) queue.offer(pNode.left);
                if (pNode.right != null) queue.offer(pNode.right);
                level.add(pNode.val);
            }
            ans.add(0, level);
        }
        return ans;
    }
```

