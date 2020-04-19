---
title: Leetcode508. Most Frequent Subtree Sum
tags:
  - 二叉树
comments: true
date: 2018-04-03 20:11:23
categories: Leetcode题解
---
求二叉树中出现频率最高的子树和。

<!-- more -->

### 508. Most Frequent Subtree Sum

Given the root of a tree, you are asked to find the most frequent subtree sum. The subtree sum of a node is defined as the sum of all the node values formed by the subtree rooted at that node (including the node itself). So what is the most frequent subtree sum value? If there is a tie, return all the values with the highest frequency in any order.

**Examples 1**
Input:

```
  5
 /  \
2   -3
```

return [2, -3, 4], since all the values happen only once, return all of them in any order.

**Examples 2**
Input:

```
  5
 /  \
2   -5
```

return [2], since 2 happens twice, however -5 only occur once.

**Note:** You may assume the sum of values in any subtree is in the range of 32-bit signed integer.



算法：用后序遍历算法，自底向上遍历二叉树，用HashMap记录每个sum出现的频率，记录最高频率，统计完后选出出现频率最高的sum。

```java
//HashMap中key代表sum值，value是该sum出现的次数
private Map<Integer, Integer> map = new HashMap<>();
private int maxCount = Integer.MIN_VALUE;
public int[] findFrequentTreeSum(TreeNode root) {
    if (root == null) return new int[0];
    getSum(root);
    List<Integer> list = new ArrayList<>();
    for (Integer key : map.keySet()) {
        if (map.get(key) == maxCount) list.add(key);
    }
    int[] ans = new int[list.size()];
    for (int i = 0; i < list.size(); i++) ans[i] = list.get(i);
    return ans;
}
public int getSum(TreeNode root) {
    if (root == null) return 0;
    int left = getSum(root.left);
    int right = getSum(root.right);
    int sum = left + right + root.val;
    if (map.get(sum) == null) map.put(sum, 1);
    map.put(sum, map.get(sum) + 1);
    if (map.get(sum) > maxCount) maxCount = map.get(sum);
    return sum;
}
```

