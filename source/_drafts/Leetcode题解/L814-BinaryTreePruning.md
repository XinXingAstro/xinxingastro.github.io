---
title: Leetcode814. Binary Tree Pruning
tags:
  - 二叉树
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-20 11:49:03
categories: Leetcode题解
---

按照题设条件对一个0 1二叉树进行剪枝。

题设条件：如果一个节点及其子节点中不包含1，则将其剪掉。

<!-- more -->

---

### 814. Binary Tree Pruning

We are given the head node `root` of a binary tree, where additionally every node's value is either a 0 or a 1.

Return the same tree where every subtree (of the given tree) not containing a 1 has been removed.

(Recall that the subtree of a node X is X, plus every node that is a descendant of X.)

```
Example 1:
Input: [1,null,0,0,1]
Output: [1,null,0,null,1]
 
Explanation: 
Only the red nodes satisfy the property "every subtree not containing a 1".
The diagram on the right represents the answer.
```
![L81401-9513649](/images/L81401.png)

```
Example 2:
Input: [1,0,1,0,0,0,1]
Output: [1,null,1,null,1]
```
![L81402](/images/L81402.png)

```
Example 3:
Input: [1,1,0,1,1,0,1,0]
Output: [1,1,0,1,1,null,1]
```
![L81403-9513834](/images/L81403.png)

**Note:** 

- The binary tree will have at most `100 nodes`.
- The value of each node will only be `0` or `1`.

---

#### 算法：递归剪枝

按照题设条件递归剪枝，其中剪枝算法的结构是固定写法。

```java
public TreeNode pruneTree(TreeNode root) {
    if (root == null)
        return null;
    return containOne(root) ? root : null;
}
public boolean containOne(TreeNode root) {
    if (root == null) return false;
    boolean left = containOne(root.left);
    boolean right = containOne(root.right);
    if (!left) root.left = null;
    if (!right) root.right = null;
    return root.val == 1 || left || right;
}
```