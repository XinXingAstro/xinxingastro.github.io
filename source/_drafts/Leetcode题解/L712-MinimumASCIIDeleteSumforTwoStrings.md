---
title: Leetcode712. Minimum ASCII Delete Sum for Two Strings
tags:
  - 动态规划
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-07-03 11:32:33
categories: Leetcode题解
---

给出两个字符串，计算最少删除多少ASCII码值能使两个字符串相等，删除时以字符为单位。

<!-- more -->

---

### 712. Minimum ASCII Delete Sum for Two Strings

Given two strings `s1, s2`, find the lowest ASCII sum of deleted characters to make two strings equal.

**Example 1:**

```
Input: s1 = "sea", s2 = "eat"
Output: 231
Explanation: Deleting "s" from "sea" adds the ASCII value of "s" (115) to the sum.
Deleting "t" from "eat" adds 116 to the sum.
At the end, both strings are equal, and 115 + 116 = 231 is the minimum sum possible to achieve this.
```

**Example 2:**

```
Input: s1 = "delete", s2 = "leet"
Output: 403
Explanation: Deleting "dee" from "delete" to turn the string into "let",
adds 100[d]+101[e]+101[e] to the sum.  Deleting "e" from "leet" adds 101[e] to the sum.
At the end, both strings are equal to "let", and the answer is 100+101+101+101 = 403.
If instead we turned both strings into "lee" or "eet", we would get answers of 433 or 417, which are higher.
```

**Note:**

`0 < s1.length, s2.length <= 1000`.

All elements of each string will have an ASCII value in `[97, 122]`.

---

#### 算法：动态规划

设`dp[i][j]`表示s1中以第i个字符结尾的子串和s2中以第j个字符结尾的子串，两个子串删除最少多少ASCII码值可以相等。

**初始条件**：逐个累加s1或s2中字符ASCII值，相当于s1或s2和一个空字符串进行比较。

**转移方程**：

1）如果`s1[i] == s2[j]`如果这个位置两个字符相等，则`dp[i][j] = dp[i - 1][j - 1]`。

2）如果`s1[i] != s2[j]`如果两个字符不等，则我们可以删除s1中第i个字符`A = dp[i - 1][j] + s1[i]`，或者删除s2中第j个字符`B = dp[i][j - 1] + s2[j]`，对于上述两种删法我们取其中最小值`dp[i][j] = Math.min(A, B)`。

```java
public int minimumDeleteSum(String s1, String s2) {
    char[] chr1 = s1.toCharArray();
    char[] chr2 = s2.toCharArray();
    int[][] dp = new int[chr1.length + 1][chr2.length + 1];
    for (int i = 1; i <= chr1.length; i++) {
        dp[i][0] = dp[i - 1][0] + chr1[i - 1];
    }
    for (int i = 1; i <= chr2.length; i++) {
        dp[0][i] = dp[0][i - 1] + chr2[i - 1];
    }
    for (int i = 1; i <= chr1.length; i++) {
        for (int j = 1; j <= chr2.length; j++) {
            if (chr1[i - 1] == chr2[j - 1])
                dp[i][j] = dp[i - 1][j - 1];
            else 
                dp[i][j] = Math.min(dp[i - 1][j] + chr1[i - 1], dp[i][j - 1] + chr2[j - 1]);
        }
    }
    return dp[chr1.length][chr2.length];
}
```

时间复杂度：O(M*N)

空间复杂度：O(M*N)

上述动态规划算法是自顶向下的动态规划，我们也可以将这种动态规划调整为自底向上的，基本原理是一样的，只是初始条件放在了dp表格外侧，这样取字符的时候dp表格下标i，j就对应字符串中的第i，j个字符，而不是第i-1，j-1个字符了。

```java
public int minimumDeleteSum(String s1, String s2) {
    char[] chr1 = s1.toCharArray();
    char[] chr2 = s2.toCharArray();
    int[][] dp = new int[chr1.length + 1][chr2.length + 1];
    for (int i = chr2.length - 1; i >= 0; i--) {
        dp[chr1.length][i] = dp[chr1.length][i + 1] + chr2[i];
    }
    for (int i = chr1.length - 1; i >= 0; i--) {
        dp[i][chr2.length] = dp[i + 1][chr2.length] + chr1[i];
    }
    for (int i = chr1.length - 1; i >= 0; i--) {
        for (int j = chr2.length - 1; j >= 0; j--) {
            if (chr1[i] == chr2[j])
                dp[i][j] = dp[i + 1][j + 1];
            else
                dp[i][j] = Math.min(dp[i + 1][j] + chr1[i], dp[i][j + 1] + chr2[j]);
        }
    }
    return dp[0][0];
}
```

