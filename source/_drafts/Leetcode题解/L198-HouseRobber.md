---
title: Leetcode198/213/337. House Robber I II III
tags:
  - 动态规划
comments: true
mathjax: false
date: 2018-04-13 23:30:20
categories: Leetcode题解
---

House Robber I: 求间隔子树组的最大和。子数组元素的最小间隔为1，子数组元素个数任意。

House Robbe II: 在一个首尾相邻的数组中求间隔子数组的最大和。子数组的最小间隔为1，子数组元素个数不限。相关问题：编程之美2.14 求数组子数组之和的最大值 扩展问题。

<!-- more -->

### 198. House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

**Credits:**
Special thanks to [@ifanchu](https://oj.leetcode.com/discuss/user/ifanchu) for adding this problem and creating all test cases. Also thanks to [@ts](https://oj.leetcode.com/discuss/user/ts) for adding additional test cases.



#### 算法：动态规划

**设dp[i]表示 以第i个元素结尾的数组的最大间隔子数组和。**

初始条件：dp[0] = nums[0], dp[1] = Math.max(dp[0], dp[1])

当我们计算dp[2]的时候，nums[2]可以选也可以不选，由于元素不能连续选取，所以dp[2]等于dp[1]、dp[0] + nums[2]这两者中较大的那个，同理计算dp[3]的时候，dp[3]只能等于dp[1]+nums[3]、dp[2]两者中较大那个，所以我们得到了递推公式：**dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i])**

所以我们可以得到如下代码：

```java
public int rob(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    if (nums.length == 1) return nums[0];
    int[] dp = new int[nums.length];
    dp[0] = nums[0];
    dp[1] = nums[1] > nums[0] ? nums[1] : nums[0];
    for (int i = 2; i < nums.length; i++) {
        dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i]);
    }
    return dp[nums.length - 1];
}
```

时间复杂的：O(n)；空间复杂度：O(n);

我们注意到每次计算dp[i]的时候只用到了dp[i-1]和dp[i-2]，所以上述代码的空间复杂度可以进一步优化：

```java
public int rob(int[] nums) {
    int cur = 0, pre = 0;
    for (int i : nums) {
        int tmp = cur;
        cur = Math.max(cur, pre + i); //cur向后移动一位
        pre = tmp; //pre向后移动一位
    }
    return cur;
}
```

时间复杂度：O(n)；空间复杂度：O(1)；



### 213. House Robber II

**Note:** This is an extension of [House Robber](https://leetcode.com/problems/house-robber/).

After robbing those houses on that street, the thief has found himself a new place for his thievery so that he will not get too much attention. This time, all houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, the security system for these houses remain the same as for those in the previous street. 

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

**Credits:**
Special thanks to [@Freezen](https://oj.leetcode.com/discuss/user/Freezen) for adding this problem and creating all test cases.



#### 算法：动态规划

在一个首尾相邻的数组中求间隔子数组的最大和。子数组的最小间隔为1，子数组元素个数不限。相关问题：编程之美2.14 求数组子数组之和的最大值 扩展问题。

如果数组的首尾相连，则对于首尾两个元素，只能选一个或者两个都不选，所以我们分两种情况讨论：

1. 选择首元素，不选择尾元素。rob(nums, 0, nums.length-2);
2. 选择尾元素，不选择首元素。rob(nums, 1, nums.length-1);

对于两个都不选的情况，已经包含在了1，2里面，所以最后我们从这两者中选出大的就是解。

```java
public int rob(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    if (nums.length == 1) return nums[0];
    return Math.max(rob(nums, 0, nums.length-2), rob(nums, 1, nums.length-1));
}
public int rob(int[] nums, int start, int end) {
    int cur = 0, pre = 0;
    for (int i = start; i <= end; i++) {
        int tmp = cur;
        cur = Math.max(cur, pre + nums[i]);
        pre = tmp;
    }
    return cur;
}
```



### 337. House Robber III

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

**Example 1:**

```
     3
    / \
   2   3
    \   \ 
     3   1
```

Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.

**Example 2:**

```
     3
    / \
   4   5
  / \   \ 
 1   3   1
```

Maximum amount of money the thief can rob = 4 + 5 = 9.

**Credits:**
Special thanks to [@dietpepsi](https://leetcode.com/discuss/user/dietpepsi) for adding this problem and creating all test cases.



#### 算法：递归+动态规划

**对于每个节点，设rob[1]表示从当前节点到叶子节点抢到的最大值，rob[0]表示从当前节点的左右孩子节点到叶子节点抢到的最大值。**我们用robLeft[]和robRight[]表示当前节点的左右子节点的rob数组，则有：

1. 当前节点不被抢劫，则当前节点的左右孩子节点被抢劫：rob[0] = robLeft[1] + robRight[1];
2. 当前节点被抢劫，则左右孩子节点不被抢劫，或者当前节点不被抢，这两者的最大值：rob[1] =Math.max(robLeft[0] + robRight[0] + root.val, rob[0]); 

时间复杂度：O(n)；

空间复杂度：O(n);

```java
public int rob(TreeNode root) {  
    return getSum(root)[1];  
}  
private int[] getSum(TreeNode root) {  
    int[] rob ={0, 0};  
    if(root == null) return rob;

    int[] robLeft = getSum(root.left);  
    int[] robRight = getSum(root.right);

    rob[0] = robLeft[1] + robRight[1];  
    rob[1] = Math.max(robLeft[0] + robRight[0] + root.val, rob[0]); 

    return rob;  
}
```

