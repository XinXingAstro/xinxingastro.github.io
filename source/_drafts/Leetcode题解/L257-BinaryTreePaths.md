---
title: Leetcode257. Binary Tree Paths
tags:
  - 二叉树
comments: true
date: 2018-04-01 17:26:11
categories: Leetcode题解
---
获得二叉树的所有路径，代码非常简洁。

<!-- more -->

### 257. Binary Tree Paths

Given a binary tree, return all root-to-leaf paths.

For example, given the following binary tree:

```
   1
 /   \
2     3
 \
  5
```

All root-to-leaf paths are:

```
["1->2->5", "1->3"]
```

**Credits:**
Special thanks to [@jianchao.li.fighter](https://leetcode.com/discuss/user/jianchao.li.fighter) for adding this problem and creating all test cases.



算法：基于深度优先遍历算法，遇到叶子节点就将当前路径加入结果集合。

```java
public List<String> binaryTreePaths(TreeNode root) {
    List<String> ans = new ArrayList<>();
    if (root != null) preorderWalk(root, "", ans);
    return ans;
}
public void preorderWalk(TreeNode root, String path, List<String> ans) {
    if (root.left == null && root.right == null) ans.add(path + root.val);
    if (root.left != null) preorderWalk(root.left, path + root.val + "->", ans);
    if (root.right != null) preorderWalk(root.right, path + root.val + "->", ans);
}
```

