---
title: Leetcode200. Number of Islands
tags:
  - 矩阵
  - DFS
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-09 23:41:16
categories: Leetcode题解
---

给出一个矩阵，1代表陆地，0代表水，如果一片陆地的上下左右都是水就认为这片陆地是一个岛，找出矩阵中所有岛。

<!-- more -->

---

### 200. Number of Islands

Given a 2d grid map of `'1'`s (land) and `'0'`s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

```
Input:
11110
11010
11000
00000

Output: 1
```

**Example 2:**

```
Input:
11000
11000
00100
00011

Output: 3
```

---

#### 算法：深度优先遍历

利用图的深度优先遍历算法，从一个1点开始找这个1点周围所有的1，然后把这些1改为其他符号，然后继续在矩阵中搜索1点，同时累计所有1片的个数。

其中图的深度优先遍历算法是固定写法，在[Leetcode79. Word Search](https://xinxingastro.github.io/2018/06/09/Leetcode题解/L79-WordSearch/)中也曾用到过。

```java
public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) 
        return 0;
    int count = 0;
    for (int i = 0; i < grid.length; i++)
        for (int j = 0; j < grid[0].length; j++)
            if (grid[i][j] == '1') {
                dfs(grid, i, j);
                count++;
            }
    return count;
}
public void dfs(char[][] grid, int i, int j) {
    if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] != '1') 
        return;  
    grid[i][j] = '0';
    dfs(grid, i+1, j);
    dfs(grid, i-1, j);
    dfs(grid, i, j+1);
    dfs(grid, i, j-1);
}
```

