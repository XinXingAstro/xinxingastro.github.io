---
title: Leetcode62/63. Unique Paths I II
tags:
  - 动态规划
comments: true
mathjax: true
date: 2018-04-07 15:50:58
categories: Leetcode题解
---

求到达一个方格的左右路径。

<!-- more -->

### 62. Unique Paths

A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

![img](https://leetcode.com/static/images/problemset/robot_maze.png)

Above is a 3 x 7 grid. How many possible unique paths are there?

**Note:** *m* and *n* will be at most 100.

#### 算法：动态规划

由于机器人每一次只能向下或者向右移动一格，所以每次一机器人到达一个格子只有两种可能：

1. 从上面向下移动到达这个格子。
2. 从左边向右移动到达这个格子。

因此，**设 `P[i][j]` 表示到达 `(i, j) `这个格子所有的路径条数**，则有 ：

**`P[i][j] = P[i - 1][j] + P[i][j - 1] `**

由于最左边一列没有前一列，最上面一行没有上一行，所以我们将 `P[0][j]` 和 `P[i][0]` 初始化为1。

根据上面的分析我们可以得到以下代码：

```java
int uniquePaths(int m, int n) {
    int[][] path = new int[m][n];
    for (int i = 0; i < m; i++) path[i][0] = 1;
    for (int i = 0; i < n; i++) path[0][i] = 1;
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            path[i][j] = path[i - 1][j] + path[i][j - 1];
        }
    }
    return path[m - 1][n - 1];
}
```

以上算法的时间复杂度为 $O(N^2)$ 空间复杂度为 $O(mn)$。由于我们每次更新 `path[i][j]` 的值只用到了它的左边一个元素和上面一个元素，所以没有必要维护整个 m*n 矩阵，我们只要维护两列数据就足够了，这样算法的空间复杂度就可以降到 $O(min(m, n))$ 。

```java
int uniquePaths(int m, int n) {
    if (m > n) return uniquePaths(n, m); //将大数当作列数，小数当作行数，矩阵是一个躺着的长方形
    int[] pre = new int[m];
    int[] cur = new int[m];
    for (int i = 0; i < m; i++) {
        pre[i] = 1;
        cur[i] = 1;
    }
    for (int j = 1; j < n; j++) {
        for (int i = 1; i < m; i++) {
            cur[i] = cur[i - 1] + pre[i];
        }
        swap(pre, cur);
    }
    return pre[m - 1];
}
public void swap(int[] a, int[] b) {
    int[] tmp = a;
    a = b;
    b = tmp;
}
```

仔细观察上述算法的计算过程我们可以发现 pre[] 数组也不是必要的我们可以直接在 cur[] 数组上进行计算：

```java
int uniquePaths(int m, int n) {
    if (m > n) return uniquePaths(n, m);
    int[] cur = new int[m];
    for (int i = 0; i < m; i++) cur[i] = 1;
    
    for (int j = 1; j < n; j++) {
        for (int i = 1; i < m; i++) {
            cur[i] += cur[i - 1];
        }
    }
    return cur[m - 1];
}
```



### 63. Unique Paths II

Follow up for "Unique Paths":

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as `1` and `0` respectively in the grid.

For example,

There is one obstacle in the middle of a 3x3 grid as illustrated below.

```
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
```

The total number of unique paths is `2`.

**Note:** *m* and *n* will be at most 100.

#### 算法：动态规划

根据上面的算法，遇到 `obstacleGrid[][]` 矩阵中值为1的位置时，将 `path[]` 相应位置的值设为0。上面的算法是从上到下扫描，该算法是从左到右扫描。

```java
public int uniquePathsWithObstacles(int[][] obstacleGrid) {
    int width = obstacleGrid[0].length;
    int[] dp = new int[width];
    dp[0] = 1;
    for (int[] row : obstacleGrid) {
        for (int j = 0; j < width; j++) {
            if (row[j] == 1) dp[j] = 0;
            else if (j > 0)
                dp[j] += dp[j - 1];
        }
    }
    return dp[width - 1];
}
```

