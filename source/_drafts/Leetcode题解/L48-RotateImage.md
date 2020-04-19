---
title: Leetcode48. Rotate Image
tags:
  - 矩阵
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-08 16:32:42
categories: Leetcode题解
---

将矩阵顺时针旋转90度。

<!-- more -->

---

### 48. Rotate Image

You are given an *n* x *n* 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

**Note:**

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

**Example 1:**

```
Given input matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

rotate the input matrix in-place such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

**Example 2:**

```
Given input matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

rotate the input matrix in-place such that it becomes:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```

---

#### 算法：主对角线翻转+垂直中线翻转

对矩阵顺时针旋转90度的操作可以分解为两步：

1）沿主对角线翻转矩阵：

```
[1,2,3]	   [1,4,7]
[4,5,6] => [2,5,8]
[7,8,9]    [3,6,9]
```

2）沿垂直中线翻转矩阵

```
[1,4,7]    [7,4,1]
[2,5,8] => [8,5,2]
[3,6,9]    [9,6,3]
```

```java
public void rotate(int[][] matrix) {
    if (matrix == null) return;
    int n = matrix.length;
    for (int i = 0; i <= (n-2); i++) {
        for (int j = (i+1); j <= (n-1); j++) {
            // swap(matrix[i][j], matrix[j][i]);
            int tmp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = tmp;
        }
    }
    for (int i = 0; i <= (n-1); i++) {
        for (int j = 0; j < (n >> 1); j++) {
            // swap(matrix[i][j], matrix[i][n-1-j]);
            int tmp = matrix[i][j];
            matrix[i][j] = matrix[i][n-1-j];
            matrix[i][n-1-j] = tmp;
        }
    }
}
```

时间复杂度：O(n^2)

空间复杂度：O(1)