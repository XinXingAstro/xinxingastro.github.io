---
title: Leetcode494. Target Sum
tags:
  - 动态规划
comments: true
mathjax: true
date: 2018-04-20 19:51:36
categories: Leetcode题解
typora-copy-images-to: ../../images
typora-root-url: ../../../source
---

给出一个数组，数组中的每个元素可以分配`+`或者`-`，找到让分配后的数组和为S的所有分配方法。

相关题目：[Leetcode416. Partition Equal Subset Sum](https://xinxingastro.github.io/2018/04/20/Leetcode416-PartitionEqualSubsetSum/)

<!-- more -->

### 494. Target Sum

You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols `+` and `-`. For each integer, you should choose one from `+` and `-` as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S. 

**Example 1:**

```
Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5
Explanation: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
```

**Note:**

1. The length of the given array is positive and will not exceed 20. 
2. The sum of elements in the given array will not exceed 1000.
3. Your output answer is guaranteed to be fitted in a 32-bit integer.

### 算法1: 暴力解法

基于递归的暴力枚举法，我们要让每个数都为+或者-，找出那种指定方法能让数组最后的和为S。

```java
int count = 0;
public int findTargetSumWays(int[] nums, int S) {
    calculate(nums, 0, 0, S);
    return count;
}
public void calculate(int[] nums, int i, int sum, int S) {
    if (i == nums.length) {
        if (sum == S) count++;
    } else {
        calculate(nums, i + 1, sum + nums[i], S);
        calculate(nums, i + 1, sum - nums[i], S);
    }
}
```

由于每个数都可以是+或者-，所以时间复杂度：O(2^n)。

由于进行n重递归计算，所以空间复杂度为：O(n)。

Leetcode: 704ms



### 算法2: 带备忘录的递归

上面的算法存在很多多余计算，为了消除多余计算，我们将每一个`calculate(nums, i, sum, S)`的结果存储在`memo[i][sum+1000]`里，这里的加1000是由于题目中给出，sum的值不可能大于1000，为了让所有的sum都为正数，我们将全部sum的值向右移1000。

```java
int count = 0;
public int findTargetSumWays(int[] nums, int S) {
    int[][] memo = new int[nums.length][2001];
    for (int[] row : memo) {
        Arrays.fill(row, Integer.MIN_VALUE);
    }
    return calculate(nums, 0, 0, S, memo);
}
public int calculate(int[] nums, int i, int sum, int S, int[][] memo) {
    if (i == nums.length) {
        if (sum == S) return 1;
        else return 0;
    } else {
        if (memo[i][sum + 1000] != Integer.MIN_VALUE) {
            return memo[i][sum + 1000];
        }
        int add = calculate(nums, i + 1, sum + nums[i], S, memo);
        int subtract = calculate(nums, i + 1, sum - nums[i], S, memo);
        memo[i][sum + 1000] = add + subtract;
        return memo[i][sum + 1000];
    }
}
```

时间复杂度：O(RangOfSum * nums.length);

空间复杂度：O(n). 递归树的深度为n。

Leetcode: 26ms



### *算法3: 动态规划

原问题相当于这样一个问题，在原数组中找到一个子集，如果这个子集中的数字为正，其他的数字为负，那么这两部分的和就等于`target`，我们使用`P`表示正数子集，`N`表示负数子集。

例如：给定数组`nums = [1, 2, 3, 4, 5]`和`target = 3`一个可能的解是`+1-2+3-4+5 = 3`，这里正数子集是`P = [1, 3, 5]`负数子集是`N = [2, 4]`。

该问题可以被转化为一个子集之和的问题：

```
sum(P) - sum(N) = target
sum(P) + sum(N) + sum(P) - sum(N) = target + sum(P) + sum(N)
2 * sum(P) = target + sum(nums)
```

所以原问题就被转换为了如下子集之和的问题：

从`nums`中找到一个子集`P`这个子集满足：`sum(P) = (target + sum(nums)) / 2`

以上公式证明`target + sum(nums)`必须是偶数。

对于在数组中查找和为一个数的子集的个数的算法，可以参考[Leetcode416. Partition Equal Subset Sum](https://xinxingastro.github.io/2018/04/20/Leetcode416-PartitionEqualSubsetSum/)

**设dp[i]表示，对于数组中前几个数（从第0个数到当前的数），和为i的子集的个数。**

例：找到让数组`nums = [1, 2, 3]`的和为`s = 2`的所有分配方法，则`sum = 6`, `(s + sum) / 2 = 4`所以原问题转换为在数组`nums`中找到所有和为4的子集。dp[]数组计算过程如下图所示：

![494dp数组](/images/494dp数组.png)

```java
public int findTargetSumWays(int[] nums, int s) {
    int sum = 0;
    for (int n : nums)
        sum += n;
    return sum < s || (s + sum) % 2 > 0 ? 0 : subsetSum(nums, (s + sum)>>1); 
}
public int subsetSum(int[] nums, int s) {
    int[] dp = new int[s + 1]; 
    dp[0] = 1;
    for (int n : nums)
        for (int i = s; i >= n; i--)
            dp[i] += dp[i - n]; 
    return dp[s];
} 
```

时间复杂度：O(ns); n代表数组的长度

空间复杂度：O(s)