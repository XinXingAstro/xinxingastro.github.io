---
title: Leetcode463. Island Perimeter
tags:
  - DFS
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-29 16:43:04
categories: Leetcode题解
---

给出一个矩阵，1代表陆地，0代表水，矩阵里面的1连成一个小岛，计算小岛的边界长度。

<!-- more -->

---

### 463. Island Perimeter

You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water. Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells). The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

**Example:**

```
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]

Answer: 16
Explanation: The perimeter is the 16 yellow stripes in the image below:
```

![L46301](/images/L46301.png)

---

#### 算法1: DFS

我们可以用图的深度优先遍历算法遍历小岛的每一块土地，遍历到一块儿土地的时候，判断它四周是水还是陆地，在此过程中累加小岛的边界长度。

```java
private int res;
public int islandPerimeter(int[][] grid) {
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] == 1) {
                DFS(grid, i, j);
                return res;
            }
        }
    }
    return res;
}
public void DFS(int[][] grid, int i, int j) {
    if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] != 1)
        return;
    grid[i][j] = -1;
    if (i - 1 < 0 || grid[i - 1][j] == 0 ) // up
        res++;
    if (i + 1 >= grid.length || grid[i + 1][j] == 0) // down
        res++;
    if (j - 1 < 0 || grid[i][j - 1] == 0) // left
        res++;
    if (j + 1 >= grid[0].length || grid[i][j + 1] == 0) // right
        res++;
    DFS(grid, i + 1, j);
    DFS(grid, i - 1, j);
    DFS(grid, i, j + 1);
    DFS(grid, i, j - 1);
}
```

时间复杂度：O(n)

空间复杂度：O(n)

#### 算法2: 计算总边数和邻边的差

每一块土地有4条表，我们可以先统计处土地的数量计算出总边数，由于相接的凉快土地的邻边被计算了两次，我们只有减去这个邻边数就可以计算出小岛的外边长。

```java
public int islandPerimeter(int[][] grid) {
    int res = 0;
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] == 1) {
                res += 4;
                if (i < grid.length - 1 && grid[i + 1][j] == 1) res -= 2;
                if (j < grid[0].length - 1 && grid[i][j + 1] == 1) res -= 2;
            }
        }
    }
    return res;
}
```

数据复杂度: O(n)

空间复杂度: O(1)