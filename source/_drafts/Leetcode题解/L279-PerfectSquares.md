---
title: Leetcode279. Perfect Squares
tags:
  - 动态规划
comments: true
mathjax: false
date: 2018-04-15 16:35:06
categories: Leetcode题解
---

给定一个数，求该数最少能拆分成几个数的完全平方。

<!-- more -->

### 279. Perfect Squares

Given a positive integer *n*, find the least number of perfect square numbers (for example, `1, 4, 9, 16, ...`) which sum to *n*.

For example, given *n* = `12`, return `3` because `12 = 4 + 4 + 4`; given *n* = `13`, return `2` because `13 = 4 + 9`.

**Credits:**
Special thanks to [@jianchao.li.fighter](https://leetcode.com/discuss/user/jianchao.li.fighter) for adding this problem and creating all test cases.



#### 算法1: 动态规划

**设dp(i)表示数字i的最小完全平方数**，我们自底向上遍历，计算dp[1]到dp[n]。对于每一个i，我们用从1开始的完全平方数取拆分i，看i最少能被多少个完全平方数拆分。

```java
public int numSquares(int n) {
    int[] dp = new int[n + 1];
    //由于下面要计算最小的拆分数，所以将数组里所有数字初始化为最大数
    Arrays.fill(dp, Integer.MAX_VALUE); 
    dp[0] = 0;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j * j <= i; j++) {
            dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
        }
    }
    return dp[n];
}
```



#### 算法2：数学方法

数学知识补充：

>[拉格朗日四平方和定理（Lagrange's four-square theorem）](https://zh.wikipedia.org/wiki/四平方和定理)：每个正数均可表示为4个整数的平方和。

有拉格朗日四平方定理可知，该题的答案只有1，2，3，4这四个。对于一个数，我们分这几种情况讨论：

1. 如果这个数本身就是一个完全平方数，这返回1。
2. 如果一个数对4取余等于0，则可以将所有4因子去除，化简后再进行计算结果不变。
3. 如果一个数对8取余等于7，则这个数一定可以分为4个平方数，直接返回4。

对这个数进行以上几步处理后，我们就得从1开始逐个检查，看这个数可以被几个平方数拆分，遍历的范围是从1到n的开方。如果可以被两个平方数拆分，则返回2。否则只能被三个平方数拆分，返回3。

```java
public int numSquares(int n) {
    if (isSquare(n)) return 1;

    while ((n&3) == 0) n >>=2; //while(n % 4 == 0) n /= 4;
    if ((n&7) == 7) return 4; //if (n % 8 == 7) return 4
    int sqrt = (int)Math.sqrt(n);
    for (int i = 1; i <= sqrt; i++) 
        if (isSquare(n - i*i)) return 2;
    return 3;
}

private boolean isSquare(int n) {
    int sqrt = (int)Math.sqrt(n);
    return (sqrt * sqrt) == n;
}
```

