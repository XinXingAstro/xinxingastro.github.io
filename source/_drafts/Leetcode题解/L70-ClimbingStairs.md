---
title: Leetcode70. Climbing Stairs
tags:
  - 动态规划
comments: true
mathjax: true
date: 2018-04-08 11:11:18
categories: Leetcode题解
---

找出爬楼梯的所有方法。

<!-- more -->

### 70. Climbing Stairs

You are climbing a stair case. It takes *n* steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

**Note:** Given *n* will be a positive integer.

**Example 1:**

```
Input: 2
Output:  2
Explanation:  There are two ways to climb to the top.

1. 1 step + 1 step
2. 2 steps
```

**Example 2:**

```
Input: 3
Output:  3
Explanation:  There are three ways to climb to the top.

1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

 

#### 算法1：动态规划

**设 `f(n)` 表示爬 n 阶台阶的方法数。**则有 `f(1) = 1` , `f(2) = 2` ，当 n > 2 时，第一次走的时候就有两种选择：

1. 第一次走一阶，则走 n 阶的方法就是 `f(n - 1)`
2. 第一次走两阶，则走 n 阶的方法就是 `f(n - 2)` 

所以有，当 n > 2 时，`f(n) = f(n - 1) + f(n - 2)` ，这实际上时一个Fibonacci数列。

时间复杂度：O(n)；空间复杂度：O(n)；

```java
public int climbStairs(int n) {
    if (n == 1) return 1;
    int[] dp = new int[n + 1];
    dp[1] = 1;
    dp[2] = 2;
    for (int i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
}
```

根据上面算法的计算过程，计算 f(n) 只需要 f(n - 1) 和 f(n - 2) 所以没有必要维护整个 dp[] 数组，只需要两个变量足矣。

时间复杂度：O(n)；空间复杂度：O(1)；

```java
public int climbStairs(int n) {
    if (n == 1) return 1;
    int dp1 = 1;
    int dp2 = 2;
    int dp3;
    for (int i = 3; i <= n; i++) {
        dp3 = dp1 + dp2;
        dp1 = dp2;
        dp2 = dp3;
    }
    return dp2;
}
```



#### 算法2: 矩阵相乘

这个方法需要用到一个计算 Fibonacci 数列的数学公式：

$\begin{bmatrix} f(n) & f(n - 1) \\ f(n - 1) & f(n - 2) \end{bmatrix} = \begin{bmatrix} 1 & 1 \\ 1 & 0 \end{bmatrix}^{n - 1}$

有了上面的公式，我们只需要求得 $\begin{bmatrix} 1 & 1 \\ 1 & 0 \end{bmatrix}^{n - 1}$ 就可以得到 f(n) ，我们可以用分治策略自底向上求矩阵的n次方：

$a^n = \begin{cases} a^{n/2} \times a^{n/2} & {n为偶数} \\ a^{(n-1)/2} \times a^{(n-1)/2} & {n为基数} \end{cases}$

时间复杂度：O(logn)；空间复杂度：O(1)；

```java
public int climbStairs(int n) {
    int[][] q = {{1, 1}, {1, 0}};
    int[][] res = pow(q, n);
    return res[0][0];
}
public int[][] pow(int[][] a, int n) {
    int[][] ret = {{1, 0},{0, 1}}; //单位矩阵
    while (n > 0) {
        if ((n & 1) == 1) {
            ret = multiply(ret, a);
        }
        n >>= 1;
        a = multiply(a, a);
    }
    return ret;
}
public int[][] multiply(int[][] a, int[][] b) {
    int[][] c = new int[2][2];
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 2; j++) {
            c[i][j] = a[i][0] * b[0][j] + a[i][1] * b[1][j];
        }
    }
    return c;
}
```



#### 算法3：用 Fibonacci 计算公式

$F_n = \frac{1}{\sqrt5} \times \begin{bmatrix} \begin{pmatrix} \frac{1 + \sqrt5}{2}\end{pmatrix} ^ n -  \begin{pmatrix} \frac{1-\sqrt5}{2} \end{pmatrix} ^ n \end{bmatrix}$

F0 = 1, F1 = 1, F2 = 2, 所以该题应该是Fn+1

时间复杂度：O(log(n))；由于 Math.pow 的时间复杂度是 log(n) 

空间复杂度：O(1)

```java
public int climbStairs(int n) {
    double sqrt5 = Math.sqrt(5);
    double fibn = Math.pow((1 + sqrt5)/2, n+1) - Math.pow((1 - sqrt5)/2, n+1);
    return (int)(fibn/sqrt5);
}
```

