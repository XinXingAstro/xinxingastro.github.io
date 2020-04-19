---
title: Leetcode115. Distinct Subsequences
tags:
  - 动态规划
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-05-10 23:58:08
categories: Leetcode题解
---

给出两个字符串S和T，问S所有子序列中有多少和T相等。

<!-- more -->

---

### 115. Distinct Subsequences

Given a string **S** and a string **T**, count the number of distinct subsequences of **S** which equals **T**.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, `"ACE"` is a subsequence of `"ABCDE"` while `"AEC"` is not).

**Example 1:**

```
Input: S = "rabbbit", T = "rabbit"
Output: 3
Explanation:

As shown below, there are 3 ways you can generate "rabbit" from S.
(The caret symbol ^ means the chosen letters)

rabbbit
^^^^ ^^
rabbbit
^^ ^^^^
rabbbit
^^^ ^^^
```

**Example 2:**

```
Input: S = "babgbag", T = "bag"
Output: 5
Explanation:

As shown below, there are 5 ways you can generate "bag" from S.
(The caret symbol ^ means the chosen letters)

babgbag
^^ ^
babgbag
^^    ^
babgbag
^    ^^
babgbag
  ^  ^^
babgbag
    ^^^
```

#### 算法：动态规划

**设`dp[i][j]`表示`S.subString(0, i)`的子序列中有多少和`T.subString(0, j)`相等。**

对于S和T中的每一对字符都有相等和不等两种情况：

1. `S.charAt(i) == T.charAt(j)`

   这种情况下，我么在计算S中的子序列时，对于第i个字符，我们可以选入序列，也可以不选。如果将第i个元素选入序列，则`dp[i][j] = dp[i-1][j-1]`，如果不选第i个字符进入序列，则`dp[i][j] = dp[i-1][j]`，综合以上两种情况：**`dp[i][j] = dp[i-1][j-1] + dp[i-1][j]`**。

2. `S.charAt(i) != T.charAt(j)`

   这种情况下由于两个字符不等，则S中第i个字符肯定不能进入序列，则：**`dp[i][j] = dp[i-1][j]`**

**初始条件：**

S和T前面应该各有一个空字符表示初始条件，`dp[i][0] = 1`表示当T是空字符是S只有空序列和它对应。

2D的状态转移矩阵如下所示：

| S        T | 空字符 | b     | a    | g     |
| ---------- | ------ | ----- | ---- | ----- |
| 空字符     | 1      | 0     | 0    | 0     |
| b          | 1      | 1     | 0    | 0     |
| a          | 1      | 1     | 1    | 0     |
| b          | 1      | 1+1=2 | 1    | 0     |
| g          | 1      | 2     | 1    | 1+0=1 |
| b          | 1      | 1+2=3 | 1    | 1     |
| a          | 1      | 3     | 4    | 1     |
| g          | 1      | 3     | 4    | 5     |

```java
public int numDistinct(String s, String t) {
    if (t == null) return 1;
    if (s == null) return 0;
    int tLen = t.length();
    int sLen = s.length();
    int[][] dp = new int[sLen + 1][tLen + 1];
    for (int i = 0; i <= sLen; i++) {
        dp[i][0] = 1;
    }
    for (int i = 1; i <= sLen; i++) {
        for (int j = 1; j <= tLen; j++) {
            if (s.charAt(i-1) == t.charAt(j-1)) {
                dp[i][j] = dp[i-1][j-1] + dp[i-1][j];
            } else {
                dp[i][j] = dp[i-1][j];
            }
        }
    }
    return dp[sLen][tLen];
}
```

#### 算法2: 使用Hash表

```java
public int numDistinct(String s, String t){
    final int N = t.length();
    int[] dists = new int[N + 1];
    dists[N] = 1;
    int[] firsts = new int[128];
    Arrays.fill(firsts,-1);
    int[] nexts = new int[N];
    for(int i=N-1; i>=0; i--){
        int c= t.charAt(i);
        nexts[i] = firsts[c];
        firsts[c] = i;
    }
    for (int i = s.length() - 1; i >= 0; i--) {
        char c = s.charAt(i);
        for(int j=firsts[c];j>=0;j=nexts[j])
            dists[j] += dists[j+1];
    }
    return dists[0];
}
```

