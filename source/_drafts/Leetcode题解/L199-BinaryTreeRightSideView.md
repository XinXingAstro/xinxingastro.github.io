---
title: Leetcode199. Binary Tree Right Side View
tags:
  - 二叉树
comments: true
date: 2018-03-31 16:03:53
categories: Leetcode题解
---
二叉树的右视图。

<!-- more -->

Given a binary tree, imagine yourself standing on the *right* side of it, return the values of the nodes you can see ordered from top to bottom.

For example:
Given the following binary tree,

```
   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```

You should return `[1, 3, 4]`.

**Credits:**
Special thanks to [@amrsaqr](https://leetcode.com/discuss/user/amrsaqr) for adding this problem and creating all test cases.



算法1: 基于二叉树的层次遍历，存储每一层第一个节点：

```java
public List<Integer> rightSideView(TreeNode root) {
    if (root == null) return new ArrayList<>();
    ArrayDeque<TreeNode> queue = new ArrayDeque<>();
    ArrayList<Integer> ans = new ArrayList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        ans.add(queue.peek().val);
        for (int i = 0; i < size; i++) {
            TreeNode p = queue.poll();
            if (p.right != null) queue.offer(p.right);
            if (p.left != null) queue.offer(p.left);
        }
    }
    return ans;
}
```



算法2: 递归层次遍历：

```java
public List<Integer> rightSideView(TreeNode root) {
    if (root == null) return new ArrayList<>();
    ArrayList<Integer> ans = new ArrayList<>();
    levelWalk(root, ans, 0);
    return ans;
}
public void levelWalk(TreeNode root, ArrayList<Integer> ans, int depth) {
    if (root == null) return;
    if (depth == ans.size()) ans.add(root.val);
    levelWalk(root.right, ans, depth+1);
    levelWalk(root.left, ans, depth+1);

}
```

