---
title: Leetcode653. Two Sum IV - Input is a BST
tags:
  - 二叉树
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-30 21:15:05
categories: Leetcode题解
---

给出一个搜索二叉树和一个值k，如果能在搜索二叉树中找到两个节点的的值之和等于k，则返回true，否则返回false。

<!-- more -->

---

### 653. Two Sum IV - Input is a BST

Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that their sum is equal to the given target.

**Example 1:**

```
Input: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 9

Output: True
```

**Example 2:**

```
Input: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 28

Output: False
```

---

#### 算法：转换问题为在排序数组中找两个数和等于k

由于题目给出的是搜索二叉树，根据搜索二叉树的特性，我们可以使用中序遍历BST各节点得到一个排序数组，这样就可以将原问题转换为在一个排序数组中找两个数，使其和等于k：

1）定义两个指针l r，分别指向数组头尾，指针指向的两元素和为sum。

2）如果sum > k，r指针向前移动。

3）如果sum < k，l指针向后移动。

4）如果sum = k，返回true，如果l >= r，返回false。

```java
private ArrayList<Integer> keyMap = new ArrayList();
public boolean findTarget(TreeNode root, int k) {
    if (root == null) return false;
    getKey(root);
    int size = keyMap.size();
    int l = 0, r = size - 1, sum = 0;
    while (l < r) {
        sum = keyMap.get(l) + keyMap.get(r);
        if (sum > k) r--;
        else if (sum < k) l++;
        else return true;
    }
    return false;
}
public void getKey(TreeNode root) {
    if (root == null) return;
    getKey(root.left);
    keyMap.add(root.val);
    getKey(root.right);
}
```

时间复杂度：O(n)

空间复杂度：O(n)