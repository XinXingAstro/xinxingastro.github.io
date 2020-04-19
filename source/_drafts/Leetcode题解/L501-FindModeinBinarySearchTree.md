---
title: Leetcode501. Find Mode in Binary Search Tree
tags:
  - 二叉树
comments: true
date: 2018-04-03 17:31:45
categories: Leetcode题解
---
在**搜索二叉树**中找出 出现频率最高的节点。

<!-- more -->

### 501. Find Mode in Binary Search Tree

Given a binary search tree (BST) with duplicates, find all the [mode(s)](https://en.wikipedia.org/wiki/Mode_(statistics)) (the most frequently occurred element) in the given BST.

Assume a BST is defined as follows:

- The left subtree of a node contains only nodes with keys **less than or equal to** the node's key.
- The right subtree of a node contains only nodes with keys **greater than or equal to** the node's key.
- Both the left and right subtrees must also be binary search trees.

For example:
Given BST `[1,null,2,2]`,

```
   1
    \
     2
    /
   2
```

return `[2]`.

**Note:** If a tree has more than one mode, you can return them in any order.

**Follow up:** Could you do that without using any extra space? (Assume that the implicit stack space incurred due to recursion does not count).

算法：由于该题的二叉树中有重复节点，并且该树是搜索二叉树，由于搜索二叉树的中序遍历序列应该是递增序列，所以，所有重复的元素，在中序遍历序列中都是挨着的，所以该题就转化成了：在一个递增的有重复元素的序列中，找出出现频率最高的元素(出现频率最高的元素可能有多个)。

```java
//解该题最重要的一点是要知道，如果搜索二叉树中存在重复元素，则该树的中序遍历序列中，相同元素肯定都挨在一块儿
private int curNodeVal;
private int curNodeCount = 0;
private int maxCount = 1;
public int[] findMode(TreeNode root) {
    if (root == null) return new int[0];
    ArrayList<Integer> modeList = new ArrayList<>();
    inorderWalk(root, modeList);
    int[] modes = new int[modeList.size()];
    for (int i = 0; i < modeList.size(); i++) modes[i] = modeList.get(i);
    return modes;
}
private void inorderWalk(TreeNode root, ArrayList<Integer> modeList) {
    if (root == null) return;
    inorderWalk(root.left, modeList);
    if (root.val == curNodeVal) {
        curNodeCount++;
    } else {
        curNodeVal = root.val;
        curNodeCount = 1;
    }
    if (curNodeCount == maxCount) {
        modeList.add(curNodeVal);
    }
    if (curNodeCount > maxCount) {
        maxCount = curNodeCount;
        modeList.clear();
        modeList.add(curNodeVal);
    }
    inorderWalk(root.right, modeList);
}
```

