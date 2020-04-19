---
title: Leetcode515. Find Largest Value in Each Tree Row
tags:
  - 二叉树
comments: true
mathjax: false
date: 2018-04-06 10:37:00
categories: Leetcode题解
---

找出二叉树每层中最大的元素。

<!-- more -->

### 515. Find Largest Value in Each Tree Row

You need to find the largest value in each row of a binary tree.

**Example:**

```
Input: 

          1
         / \
        3   2
       / \   \  
      5   3   9 

Output: [1, 3, 9]
```



#### 算法：层次遍历的同时记录最大节点值

```java
public List<Integer> largestValues(TreeNode root) {
    List<Integer> ans = new ArrayList<>();
    if (root == null) return ans;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < size; i++) {
            TreeNode p = queue.poll();
            if (p.val > max) max = p.val; 
            if (p.left != null) queue.offer(p.left);
            if (p.right != null) queue.offer(p.right);
        }
        ans.add(max);
    }
    return ans;
}
```