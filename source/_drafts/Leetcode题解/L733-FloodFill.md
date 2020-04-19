---
title: Leetcode733. Flood Fill
tags:
  - DFS
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-07-01 10:57:15
categories: Leetcode题解
---

输入一个矩阵表示一个图片，在这个图片上使用flood fill(类似于颜料桶的方式)对一片相同颜色的区域进行染色。

<!-- more -->

---

### 733. Flood Fill

An `image` is represented by a 2-D array of integers, each integer representing the pixel value of the image (from 0 to 65535).

Given a coordinate `(sr, sc)` representing the starting pixel (row and column) of the flood fill, and a pixel value `newColor`, "flood fill" the image.

To perform a "flood fill", consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color as the starting pixel), and so on. Replace the color of all of the aforementioned pixels with the newColor.

At the end, return the modified image.

**Example 1:**

```
Input: 
image = [[1,1,1],[1,1,0],[1,0,1]]
sr = 1, sc = 1, newColor = 2
Output: [[2,2,2],[2,2,0],[2,0,1]]
Explanation: 
From the center of the image (with position (sr, sc) = (1, 1)), all pixels connected 
by a path of the same color as the starting pixel are colored with the new color.
Note the bottom corner is not colored 2, because it is not 4-directionally connected
to the starting pixel.
```

**Note:**

The length of `image` and `image[0]` will be in the range `[1, 50]`.

The given starting pixel will satisfy `0 <= sr < image.length` and `0 <= sc < image[0].length`.

The value of each color in `image[i][j]` and `newColor` will be an integer in `[0, 65535]`.

---

#### 算法：DFS

这是典型的应用DFS算法的场景。注意在使用DFS算法进行染色之前要判断新颜色和旧颜色是否相同，如果相同就不用进行染色，如果不进行这步判断会出现StackOverFlow异常。

```java
private int oldColor;
public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
    oldColor = image[sr][sc];
    if (oldColor != newColor)
        DFS(image, sr, sc, newColor);
    return image;
}
public void DFS(int[][] image, int i, int j, int newColor) {
    if (i < 0 || i >= image.length || j < 0 || j >= image[0].length || image[i][j] != oldColor)
        return;
    image[i][j] = newColor;
    DFS(image, i + 1, j, newColor);
    DFS(image, i - 1, j, newColor);
    DFS(image, i, j + 1, newColor);
    DFS(image, i, j - 1, newColor);
}
```

时间复杂度：O(n)

空间复杂度：O(1)