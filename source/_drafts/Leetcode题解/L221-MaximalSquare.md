---
title: Leetcode221-MaximalSquare
tags:
  - 动态规划
comments: true
mathjax: false
date: 2018-04-15 13:06:17
categories: Leetcode题解
---

求最大全1子矩阵(正方形矩阵)。相关问题：编程之美2.15 子数组之和的最大值（二维）；面试指南：子矩阵的最大累加和问题。

<!-- more -->

### 221. Maximal Square

Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

For example, given the following matrix:

```
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```

Return 4.

**Credits:**
Special thanks to [@Freezen](https://oj.leetcode.com/discuss/user/Freezen) for adding this problem and creating all test cases.



#### 算法1: 枚举

枚举所有子矩阵，找出最大的全是1的**正方形**子矩阵。

我们从左上角元素开始扫描，当扫描到1的时候，我们尝试找出包括这个1的最大正方形矩阵，因此我们临时增加一行或者一列，扫描其中是否全部都是1，如果出现一个0，则跳出循环。

```java
public int maximalSquare(char[][] matrix) {
    int rows = matrix.length, cols = rows > 0 ? matrix[0].length : 0;
    int maxsqlen = 0;
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (matrix[i][j] == '1') {
                int sqlen = 1;
                boolean flag = true;//flag表示是否遇到0
                while (i + sqlen < rows && j + sqlen < cols && flag) {
                    //扫描下面新加的一行是否全为1，如果遇到0跳出循环
                    for (int k = j; k <= sqlen + j; k++) {
                        if (matrix[i + sqlen][k] == '0') {
                            flag = false;
                            break;
                        }
                    }
                    //扫描新加的一列是否全为1，如果遇到0跳出循环
                    for (int k = i; k <= sqlen + i; k++) {
                        if (matrix[k][j + sqlen] == '0') {
                            flag = false;
                            break;
                        }
                    }
                    //如果新加的行和列中都没有0，则再加一行一列
                    if (flag)
                        sqlen++;
                }
                //扫描完一个1节点后更新maxsqlen的值
                if (maxsqlen < sqlen) {
                    maxsqlen = sqlen;
                }
            }
        }
    }
    return maxsqlen * maxsqlen;
}
```

时间复杂度：$O((mn)^2)$ , Leetcode运行时间18ms

空间复杂度：O(1).



#### 算法2: 动态规划

**设dp(i, j)代表以(i, j)为右下角的最大全1正方形的边长。**还是从左上角开始扫描，对于每一个值为1的元素，我们更新dp(i, j)的值：

**dp(i, j) = Math.min(dp(i-1, j), dp(i-1, j-1), dp(i, j-1)) + 1**

同时我们记录dp(i, j)的最大值。

![Max Square](https://leetcode.com/media/original_images/221_Maximal_Square.PNG?raw=true)

```java
public int maximalSquare(char[][] matrix) {
    int rows = matrix.length, cols = rows > 0 ? matrix[0].length : 0;
    //注意这里dp矩阵要比原矩阵大一个，所以matrix[i-1][j-1]的dp值是放在dp[i][j]中的
    int[][] dp = new int[rows + 1][cols + 1]; 
    int maxsqlen = 0;
    for (int i = 1; i <= rows; i++) {
        for (int j = 1; j <= cols; j++) {
            if (matrix[i-1][j-1] == '1'){
                dp[i][j] = Math.min(Math.min(dp[i][j - 1], dp[i - 1][j]), dp[i - 1][j - 1]) + 1;
                maxsqlen = Math.max(maxsqlen, dp[i][j]);
            }
        }
    }
    return maxsqlen * maxsqlen;
}
```

时间复杂的：O(mn)

空间复杂度：O(mn)



#### 算法3: 优化空间复杂度的动态规划

算法2在计算dp(i, j)的时候只用到了(i, j)元素左上方的三个元素，所以可以将2D动态规划降低到1D。

我们初始化一个dp[]数组，里面的元素都为0，我们逐行扫描原矩阵，然后更新dp[j]的值。

**dp[j] = min(dp[j-1], dp[j], prev)**. prev代表之前的dp[j-1].

![ Max Square ](https://leetcode.com/media/original_images/221_Maximal_Square1.png?raw=true)

```java
public int maximalSquare(char[][] matrix) {
    int rows = matrix.length, cols = rows > 0 ? matrix[0].length : 0;
    //matrix[i-1][j-1]的dp值实际放在dp[j]里
    int[] dp = new int[cols + 1];
    int maxsqlen = 0, prev = 0;
    for (int i = 1; i <= rows; i++) {
        for (int j = 1; j <= cols; j++) {
            int temp = dp[j];
            if (matrix[i - 1][j - 1] == '1') {
                dp[j] = Math.min(Math.min(dp[j - 1], prev), dp[j]) + 1;
                maxsqlen = Math.max(maxsqlen, dp[j]);
            } else {
                dp[j] = 0;
            }
            prev = temp;
        }
    }
    return maxsqlen * maxsqlen;
}
```





