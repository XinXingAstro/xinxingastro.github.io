---
title: Leetcode95/96. Unique Binary Search Trees
tags:
  - 二叉树
  - 二叉搜索树
  - 动态规划
comments: true
date: 2018-03-25 21:50:43
categories: Leetcode题解
---
Leetcode 95 和 96题 Unique Binary Search Trees解法总结。

<!-- more -->

### 96. Unique Binary Search Trees

Given *n*, how many structurally unique **BST's** (binary search trees) that store values 1...*n*?

For example,
Given *n* = 3, there are a total of 5 unique BST's.

```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

设dp[k]表示从1到k这k个数字所能建立的BST的总数，假设我们已知这5个数：dp[0] = 1, dp[1] = 1, dp[2] = 2, dp[3] = 5, dp[4] = 14。当我们计算dp[5]时，我们从{1,2,3,4,5}这5个节点中依次选出每一个节点作为根：

选1为根，左子树个数为0，**{2,3,4,5}这四个节点所能组成的BST的个数和{1,2,3,4}这4个节点所能组成的BST的个数一样等于dp[4]=14**，所以右子树的个数为14，所以选1为根得到的BST的个数R1 = dp[0] * dp[4] = 14。

同理选2为根，得到的BST的个数R2 = 左子树个数 * 右子树个数 = dp[1] * dp[3] = 5

选3为根，得到BST个数R2 = dp[2] * dp[2] = 4

选4为根，得到BST个数R3 = dp[3] * dp[1] = 5

选5为根，得到BST个数R4 = dp[4] * dp[0] = 14

所以dp[5] = R1 + R2 + R3 + R4 + R5 = 42。因此我们就知道了dp[k]的计算方法。

dp[k] = $\sum_{i = 1}^{k}dp[i-1]*dp[k-i]$

```java

public int numTrees(int n) {
    int[] dp = new int[n + 1];
    dp[0] = 1;
    dp[1] = 1;
    for (int nodeNum = 2; nodeNum <= n; nodeNum++) {
        for (int rootIndex = 1; rootIndex <= nodeNum; rootIndex++) {
            dp[nodeNum] += dp[rootIndex - 1] * dp[nodeNum - rootIndex];
        }
    }
    return dp[n];
}

```


### 95. Unique Binary Search Trees II

Given an integer *n*, generate all structurally unique **BST's** (binary search trees) that store values 1...*n*.

For example,
Given *n* = 3, your program should return all 5 unique BST's shown below.

```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```



```java
public List<TreeNode> generateTrees(int n) {
    if (n <= 0) {
        return new ArrayList<TreeNode>();
    }
    return genTreeList(1,n);
}
    
private List<TreeNode> genTreeList (int start, int end) {
    List<TreeNode> list = new ArrayList<TreeNode>(); 
    if (start > end) {
        list.add(null);
    }
    for(int rootIndex = start; rootIndex <= end; rootIndex++) {
        List<TreeNode> leftList = genTreeList(start, rootIndex - 1);
        List<TreeNode> rightList = genTreeList(rootIndex + 1, end);
        for (TreeNode left : leftList) {
            for(TreeNode right : rightList) {
                TreeNode root = new TreeNode(rootIndex);
                root.left = left;
                root.right = right;
                list.add(root);
            }
        }
    }
    return list;
}
```