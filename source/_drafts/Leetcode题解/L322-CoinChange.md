---
title: Leetcode322. Coin Change
tags:
  - 动态规划
comments: true
mathjax: true
date: 2018-04-19 18:42:20
categories: Leetcode题解
---

给出一个钱数和一组硬币，计算出最少用多少硬币可以组成这个钱数，如果无法组成这个钱数，则返回-1。

<!-- more -->

### 322. Coin Change

You are given coins of different denominations and a total amount of money *amount*. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

**Example 1:**
coins = `[1, 2, 5]`, amount = `11`
return `3` (11 = 5 + 5 + 1)

**Example 2:**
coins = `[2]`, amount = `3`
return `-1`.

**Note**:
You may assume that you have an infinite number of each kind of coin.

**Credits:**
Special thanks to [@jianchao.li.fighter](https://leetcode.com/discuss/user/jianchao.li.fighter) for adding this problem and creating all test cases.

### *算法：自底向上动态规划

**设F(S)表示最少使用多少个硬币来组成钱数S, 共有n种硬币。**

$F(S) = min_{i=0…n-1}F(S - c_i) + 1\space (S-c_i \geq 0)$

$F(S) = 0, when\space S = 0$

$F(S) = -1, when\space n = 0$

下面是一个例子：

![Bottom-up approach using a table to build up the solution to F6.](https://leetcode.com/media/original_images/322_coin_change_table.png)

例如我们在计算F(3)的时候：

$F(3) = min(F(3-c_1), F(3-c_2), F(3-c_3)) + 1$

​	  $= min(F(3-1), F(3-2), F(3-3)) + 1$

​	  $= min(F(2), F(1), F(0)) + 1$

​	  $=min(1,1,0) + 1=1$

```java
public int coinChange(int[] coins, int amount) {
    int max = amount + 1;             
    int[] dp = new int[amount + 1];  
    Arrays.fill(dp, max);
    dp[0] = 0;   
    for (int i = 1; i <= amount; i++) {
        for (int j = 0; j < coins.length; j++) {
            if (coins[j] <= i) {
                dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
            }
        }
    }
    return dp[amount] > amount ? -1 : dp[amount];
}
```

时间复杂度：O(S*n) 

空间复杂度：O(S)



### 算法2：递归



```java
int min = -1;
int[] numbers = null;
public int coinChange(int[] coins, int amount) {
    if(amount == 0)	return 0;
    numbers = coins;
    Arrays.sort(numbers);

    coinChange(amount, coins.length - 1, 0);
    return min;
}

public void coinChange(int amount, int index, int num){
    int coin = numbers[index];
    if(amount % coin == 0){
        int newNum = num + amount/coin;
        if(min==-1 || newNum < min) min = newNum;
    }
    if(index == 0)	return;

    for(int i = amount/coin; i >= 0; i--){
        int newAmount = amount - numbers[index]*i;
        int newNum = num + i;
        int nextCoin = numbers[index-1];

        if(min!=-1 && newNum + (newAmount + nextCoin -1) / nextCoin >= min) break;

        coinChange(newAmount, index-1, newNum);
    }
}
```

