---
title: Leetcode72. Edit Distance
tags:
  - 动态规划
comments: true
mathjax: false
date: 2018-04-08 16:43:00
categories: Leetcode题解
---

将一个字符串转换为另一个字符串的最小步数。

<!-- more -->

### 72. Edit Distance

Given two words *word1* and *word2*, find the minimum number of steps required to convert *word1* to *word2*. (each operation is counted as 1 step.)

You have the following 3 operations permitted on a word:

a) Insert a character
b) Delete a character
c) Replace a character



#### 算法：动态规划

**设 `f[i][j]` 表示将字符串 `word1[0, i-1]` 转换为 `word2[0, j-1]` 所使用的最小编辑次数。**

为了计算 `f[i][j]` 我们比较 `word1` 和 `word2` 的最后一个字符，即 `word1[i-1]` 和 `word2[j-1]`:

1. 如果 `word1[i-1] == word2[j-1]`，那么我们不需要进行任何操作，`f[i][j] = f[i-1][j-1]`

2. 如果 `word1[i-1] != word2[j-1]` ，那么我们要对 word1 做增删改三种操作：

   1). 如果我么将 `word1[i-1]` 替换为 `word2[j-1]` ，那么要将 `word1[0, i-1]` 转换为 `word2[0, j-1]` ，我们就要先将 `word1[0, i-2]` 转换为 `word2[0, j-2]` ，然后将 `word1[i-1]` 替换为 `word2[j-1]` ，即 `f[i][j] = f[i-1][j-1] + 1`

   2). 如果我们在 `word1[i-1]` 后面插入 `word2[j-1]` ，那么要将 `word1[0, i-1]` 转换为 `word2[0, j-1]` ，我们就先要将 `word1[0, i-1]` 转换为 `word2[0, j-2]` ，然后在`word1`最后加上`word2[j-1]`， 即 `f[i][j] = f[i][j-1] + 1` 

   3). 如果我们删除 `word1` 中的 `word1[i-1]` ，那么要将 `word1[0, i-1]` 转换为 `word2[0, j-1]` ，我们就要先将 `word1[0, i-2]` 转换为 `word2[0, j-1]`， 然后再删除`word1[i-1]`，即 `f[i][j] = f[i-1][j] + 1`

   最终 `f[i][j]` 应该在上面这三种情况里选出一个最小值。即 `f[i][j] = Min{f[i-1][j-1], f[i][j-1], f[i-1][j]} + 1`

边界条件：如果将一个空字符串转换成另一个字符串，则最小要执行n步插入操作，即：

1. `f[0][j] = j`
2. `f[i][0] = i`

时间复杂的：O(mn)；空间复杂的：O(mn)；

```java
public int minDistance(String word1, String word2) {
    int m = word1.length();
    int n = word2.length();
    
    int[][] dp = new int[m + 1][n + 1];
    for (int i = 0; i <= m; i++) {
        dp[i][0] = i;
    }
    for (int i = 1; i <= n; i++) {
        dp[0][i] = i;
    }
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (word1.charAt(i) == word2.charAt(j)) {
                dp[i + 1][j + 1] = dp[i][j];
            } else {
                int a = dp[i][j];
                int b = dp[i][j + 1];
                int c = dp[i + 1][j];
                dp[i + 1][j + 1] = a < b ? (a < c ? a : c) : (b < c ? b : c);
                dp[i + 1][j + 1]++;
            }
        }
    }
    return dp[m][n];
}
```

由上述计算 `dp[i][j]` 的过程可知每次更新 `dp[i][j]` 时只用到了三个值，所以我们只需要使用一个数组，和几个变量存储这些值就可以。

时间复杂的：O(mn)；空间复杂的：O(n)；

```java
public int minDistance(String word1, String word2) {
    int m = word1.length();
    int n = word2.length();
    if (m == 0 || n == 0) return Math.abs(m - n);
    int[] dp = new int[n + 1];
    for (int i = 0; i <= n; i++) {
        dp[i] = i;
    }
    for (int i = 1; i <= m; i++) {
        int pre = dp[0]; //pre = dp[i-1][j-1]
        dp[0] = i;
        for (int j = 1; j <= n; j++) {
            int temp = dp[j];
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[j] = pre;
            } else {
                //由于初始状态下，第0行和第0列的值时完全一样的，所以dp[j-1]就相当于dp[i][j-1]
                //dp[j]就相当于dp[i-1][j]，计算完后，我们将dp[i][j]的值覆盖到dp[j]
                dp[j] = Math.min(Math.min(dp[j - 1], dp[j]), pre) + 1;
            }
            pre = temp;
        }
    }
    return dp[n];
}
```

