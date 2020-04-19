---
title: Leetcode312. Burst Balloons
tags:
  - 动态规划
  - 分治法
comments: true
mathjax: true
date: 2018-04-19 17:49:33
categories: Leetcode题解
---

给出一串气球，每个气球上有一个数字，买炸一个气球得nums[left]*nums[i]\*nums[right]分，求能得到的最高分。

<!-- more -->

### 312. Burst Balloons

Given `n` balloons, indexed from `0` to `n-1`. Each balloon is painted with a number on it represented by array `nums`. You are asked to burst all the balloons. If the you burst balloon `i` you will get `nums[left] * nums[i] * nums[right]` coins. Here `left` and `right` are adjacent indices of `i`. After the burst, the `left` and `right` then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

**Note:** 
(1) You may imagine `nums[-1] = nums[n] = 1`. They are not real therefore you can not burst them.
(2) 0 ≤ `n` ≤ 500, 0 ≤ `nums[i]` ≤ 100

**Example:**

Given `[3, 1, 5, 8]`

Return `167`

```
    nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
   coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
```

**Credits:**
Special thanks to [@dietpepsi](https://leetcode.com/discuss/user/dietpepsi) for adding this problem and creating all test cases.

### 算法: 分治法 + 动态规划

**设dp(i, j)表示最后打破的气球位于i～j之间时，所能获得的最大点数。**

我们用分治法的思想分析这个问题。对于dp(i, j)，如果我们最后打破的气球位于x（即$dp_x(i, j)$），则有：

1. 最后一次获得的点数就为`nums[i-1]*nums[x]*nums[j+1]`
2. 想得到$dp_x(i,j)$的解，还需要计算两个子问题，即`dp(i, x-1)`最后打破气球在i～x-1时所能获得的最大点数和`dp(x+1, j)`最后打破的气球在x+1~j之间时所能获得的最大点数。

则我们可以得到$dp_x(i, j)$就为：

$dp_x(i, j) = dp(i, x-1) + nums[i-1]*nums[x]*nums[j+1] + dp(x+1, j)$

那么整个原问题的解就是x取i～j中所有值，计算出所有$dp_x(i,j)$中的最大值。

```java
public int maxCoins(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    if (nums.length == 1) return nums[0];
    int n = nums.length;
    int[] arr = new int[n+2];
    for (int i = 0; i < n; i++) {
        arr[i+1] = nums[i];
    }
    arr[0] = arr[n+1] = 1; //为原数组左右两边加1
    int[][] dp = new int[n+2][n+2];
    return helper(1, n, arr, dp);
}
private int helper(int i, int j, int[] nums, int[][] dp) {
    if (i > j) return 0;
    if (dp[i][j] > 0) return dp[i][j];//如果dp[i][j]已经算过则直接返回
    for (int x = i; x <= j; x++) {
        dp[i][j] = Math.max(dp[i][j], helper(i, x-1, nums, dp) 
                                    + nums[i-1]*nums[x]*nums[j+1]
                                    + helper(x+1, j, nums, dp));
    }
    return dp[i][j];
}
```





### 



