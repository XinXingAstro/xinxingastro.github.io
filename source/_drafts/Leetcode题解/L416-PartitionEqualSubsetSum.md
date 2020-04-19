---
title: Leetcode416. Partition Equal Subset Sum
tags:
  - 动态规划
comments: true
mathjax: false
date: 2018-04-20 01:34:00
categories: Leetcode题解
typora-root-url: ../../../source
typora-copy-images-to: ../../images
---

给出一组数，判断它是否能被分成和相等的两个子数组。对于数组中的数，我们可以选，也可以不选，所以是0-1背包问题。

<!-- more -->

### 416. Partition Equal Subset Sum

Given a **non-empty** array containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

**Note:**

1. Each of the array element will not exceed 100.
2. The array size will not exceed 200.

**Example 1:**

```
Input: [1, 5, 11, 5]

Output: true

Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

**Example 2:**

```
Input: [1, 2, 3, 5]

Output: false

Explanation: The array cannot be partitioned into equal sum subsets.
```

### 算法1: 动态规划

**设dp(i, j)表示对于数组中的前i个数，是否有和为j的组合。**即对于和j，前i个数是否能将其拆分。如果在前i个数中能找到一个组合，该组合的和为j，那么dp(i, j)为true，否则dp(i, j)为false。

边界条件：dp(0, 0) = true; （0可以组成和为0的组合）

转移方程：对于数组中的每一个数我们有有取和不取两种情况：

1. 如果我们不取第i个数，dp(i, j) = dp(i-1, j); 即如果我们不取第i个数，那么前i个数是否能拆分j的状态，应该和前i-1个数是否可以拆分j的状态相同。
2. 如果我么取第i个数，dp(i, j) = dp(i-1, j-nums[i]); 即如果我们取第i个数，那么前i个数是否能拆分j的状态，应该和前i-1个数是否能拆分j-nums[i]的状态相同。

注意到如果数组中所有数的和是奇数，那么它一定不能被拆分为两个和相同的子数组。

```java
public boolean canPartition(int[] nums) {
    int sum = 0;
    for (int num : nums) sum += num;
    if ((sum & 1) == 1) return false;
    sum /= 2;
    int n = nums.length;
    boolean[][] dp = new boolean[n+1][sum+1];
    for (int i = 0; i < dp.length; i++) {
        Arrays.fill(dp[i], false);
    }
    dp[0][0] = true;
    for (int i = 1; i < (n+1); i++) {
        dp[i][0] = true;
    }
    for (int j = 1; j < (sum+1); j++) {
        dp[0][j] = false;
    }
    for (int i = 1; i < (n+1); i++) {
        for (int j = 1; j < (sum+1); j++) {
            dp[i][j] = dp[i-1][j];
            if (j >= nums[i-1]){
                dp[i][j] = (dp[i][j] || dp[i-1][j-nums[i-1]]);
            }
        }
    }
    return dp[n][sum];
}
```

dp[]数组的计算过程如下所示：

![416dp数组](/images/416dp数组.png)

由上面的计算过程 我们可以将2D的dp[]数组简化为一维，由于计算某行后面的数值需要用到上一行前面的结果，所有对于sum变量应该从后向前遍历。

Leetcode: 27ms

```java
public boolean canPartition(int[] nums) {
    int sum = 0;
    for (int num : nums) sum += num;
    if ((sum & 1) == 1) return false;
    sum /= 2;
    int n = nums.length;
    boolean[] dp = new boolean[sum+1];
    Arrays.fill(dp, false);
    dp[0] = true;
    for (int num : nums) {
        for (int i = sum; i > 0; i--) {
            if (i >= num) dp[i] = dp[i] || dp[i - num];
        }
    }
    return dp[sum];
}
```



### 算法2: 递归+动态规划

Leetcode: 10ms

```java
public boolean canPartition(int[] nums) {
    if (nums.length == 0) return false;
    int sum = 0;
    for (int val: nums) sum += val;
    if((sum & 1) == 1) return false;
    Arrays.sort(nums);
    return dfs(nums, nums.length - 1, sum/2);
}
private boolean dfs(int[] nums, int i, int target){
    boolean result = false;
    if(target == 0){
        return true; // trimming branches
    } else if (target < 0) {
        return false; // trimming branches     
    } else if (i == 0){
        return false; // where to stop
    } else {
        int k = (i-1);
        while (k >= 1 && nums[k] == nums[k+1]) k--;
        result = dfs(nums, i-1, target - nums[i]) || dfs(nums, k, target);
    }
    return result;
}
```

