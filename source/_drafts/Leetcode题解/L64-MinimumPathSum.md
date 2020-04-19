---
title: Leetcode64. Minimum Path Sum
tags:
  - 动态规划
comments: true
mathjax: false
date: 2018-04-07 23:17:03
categories: Leetcode题解
---

在矩阵中找到一条路径，该路径上节点值的和最小。

<!-- more -->

### 64. Minimum Path Sum

Given a *m* x *n* grid filled with non-negative numbers, find a path from top left to bottom right which *minimizes* the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

**Example 1:**

```
[[1,3,1],
 [1,5,1],
 [4,2,1]]
```

Given the above grid map, return `7`. Because the path 1→3→1→1→1 minimizes the sum.



#### 算法：动态规划

**设 `S(i, j)` 表示在点 (i, j) 处的最小路径和**，则有 :

**`S(i, j) = Min(S(i - 1, j), S(i, j - 1)) + Value(i, j)`**

边界条件出现在最左边一列和最上遍一行，这两个位置每个点的 `S(i, j)` 就是逐个位置值的累加和。如 `grid = [1, 2, 3, 4]` 则 `S = [1, 3, 6, 10]` 。根据以上算法可以写出如下程序：

```java
public int minPathSum(int[][] grid) {
    int m = grid.length;
    int n = grid[0].length;
    int[][] sum = new int[m][n];
    sum[0][0] = grid[0][0];
    for (int i = 1; i < m; i++) {
        sum[i][0] = sum[i - 1][0] + grid[i][0];
    }
    for (int j = 1; j < n; j++) {
        sum[0][j] = sum[0][j - 1] + grid[0][j];
    }
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            sum[i][j] = Math.min(sum[i - 1][j], sum[i][j - 1]) + grid[i][j];
        }
    }
    return sum[m - 1][n - 1];
}
```

由上面算法的执行过程我们可以看到，每次我们更新 `sum[i][j]` 的值时，只用到了 `sum[i - 1][j]` 和 `sum[i][j - 1]` 。所以我们没有必要维护整个m*n矩阵，只需要维护两个数组足矣。

```java
public int minPathSum(int[][] grid) {
    int m = grid.length;
    int n = grid[0].length;
    int[] pre = new int[m];
    int[] cur = new int[m];
    pre[0] = grid[0][0];
    for (int i = 1; i < m; i++) {
        pre[i] = pre[i - 1] + grid[i][0];
    }
    for (int j = 1; j < n; j++) {
        cur[0] = pre[0] + grid[0][j];
        for (int i = 1; i < m; j++) {
            cur[i] = Math.min(cur[i - 1], pre[i]) + grid[i][j];
        }
        swap(pre, cur);
    }
    return pre[m - 1];
}
private void swap(int[] a, int[] b) {
    int[] tmp = a;
    a = b;
    b = tmp;
}
```

