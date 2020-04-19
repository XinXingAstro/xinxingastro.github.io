---
title: Leetcode746. Min Cost Climbing Stairs
tags:
  - 动态规划
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-05-10 15:16:09
categories: Leetcode题解
---

给出一个数组代表走每个台阶的花费，找出花费最少的爬到楼梯顶端的方法。

<!-- more -->

---

### 746. Min Cost Climbing Stairs

On a staircase, the `i`-th step has some non-negative cost `cost[i]` assigned (0 indexed).

Once you pay the cost, you can either climb one or two steps. You need to find minimum cost to reach the top of the floor, and you can either start from the step with index 0, or the step with index 1.

**Example 1:**

```
Input: cost = [10, 15, 20]
Output: 15
Explanation: Cheapest is start on cost[1], pay that cost and go to the top.
```

**Example 2:**

```
Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
Output: 6
Explanation: Cheapest is start on cost[0], and only step on 1s, skipping cost[3].
```

**Note:**

1. `cost` will have a length in the range `[2, 1000]`.
2. Every `cost[i]` will be an integer in the range `[0, 999]`.



#### 算法：动态规划

<font color=red>**设`dp[i]`表示到达第i个台阶时总的花费**</font>

到达第i个台阶时总的花费应该等于当前台阶的价格和到达前一个台阶或前两个台阶的价格之和。

**`dp[i] = cost[i] + Math.min(dp[i-1], dp[i-2])`**

边界条件：

`dp[0] = cost[0]`

`dp[1] = cost[1]`

根据以上分析我们可以得到如下代码：

```java
public int minCostClimbingStairs(int[] cost) {
    if (cost.length == 2) return Math.min(cost[0], cost[1]);
    int len = cost.length;
    int[] dp = new int[len];
    dp[0] = cost[0];
    dp[1] = cost[1];
    for (int i = 2; i < len; i++) {
        dp[i] = cost[i] + Math.min(dp[i - 1], dp[i - 2]);
    }
    return dp[len - 1] < dp[len - 2] ? dp[len - 1] : dp[len - 2];
}
```

时间复杂度：O(n)

空间复杂度：O(n)

由于计算dp[i]只用到了dp[i-1]和dp[i-2]，所以我们用3个变量代替数组，将空间复杂度降到O(1)。

```java
public int minCostClimbingStairs(int[] cost) {
    if (cost.length == 2) return Math.min(cost[0], cost[1]);
    int len = cost.length;
    int dp1 = cost[0];
    int dp2 = cost[1];
    int dp3;
    for (int i = 2; i < len; i++) {
        dp3 = cost[i] + Math.min(dp1, dp2);
        dp1 = dp2;
        dp2 = dp3;
    }
    return Math.min(dp1, dp2);
}
```

时间复杂度：O(n)

空间复杂度：O(1)