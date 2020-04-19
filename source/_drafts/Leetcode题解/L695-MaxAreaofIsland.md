---
title: Leetcode695. Max Area of Island
tags:
  - DFS
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-30 09:42:15
categories: Leetcode题解
---

给出一个矩阵，1代表陆地，一块陆地上下左右四个方向可以连接其他陆地形成小岛，计算出矩阵中岛的最大面积。

<!-- more -->

---

### 695. Max Area of Island

Given a non-empty 2D array `grid` of 0's and 1's, an **island** is a group of `1`'s (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Find the maximum area of an island in the given 2D array. (If there is no island, the maximum area is 0.)

**Example 1:**

```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
```

Given the above grid, return `6`. Note the answer is not 11, because the island must be connected 4-directionally.

**Example 2:**

```
[[0,0,0,0,0,0,0,0]]
```

Given the above grid, return `0`.

**Note:** The length of each dimension in the given `grid` does not exceed 50.

---

#### 算法1: DFS

我们可以使用深度优先搜索找出最大的1的连接片，在计算一片小岛中1的数量时`island`变量要声明成类变量或者变量，这样在递归返回时才能保证数量不减少。

```java
private int res, island;
public int maxAreaOfIsland(int[][] grid) {
    if (grid == null || grid.length == 0 || grid[0].length == 0)
        return 0;
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] == 1) {
                island = 0;
                DFS(grid, i, j);
            }
        }
    }
    return res;
}
public void DFS(int[][] grid, int i, int j) {
    if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] != 1) 
        return;
    grid[i][j] = -1;
    island++;
    if (res < island) res = island;
    DFS(grid, i, j + 1);
    DFS(grid, i, j - 1);
    DFS(grid, i - 1, j);
    DFS(grid, i + 1, j);
}
```

