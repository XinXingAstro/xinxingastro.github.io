---
title: Leetcode121/122/123/188/309. Best Time to Buy and Sell Stock I II III IV with Cooldown
tags:
  - 动态规划
comments: true
mathjax: true
date: 2018-04-17 20:44:10
categories: Leetcode题解
---

求股票的最佳买卖时间，以获得最大利润。

121: 买卖只进行一次，找出最大利润。相关题目：剑指offer面试题63: 股票的最大利润。

122: 不限制买卖次数，找出最大利润，不能在同一时间参与多次交易(在买第二张股票之前先要卖出手中的股票).

123: 最多可以进行两次买卖操作，找出最大利润，不能在同一时间参与多次交易。

188: 最多可以进行K次买卖操作，找出最大利润，不能在同一时间参与多次交易。

309: 不限制买卖次数，卖出股票的后一天不能再买股票，找出最大利润，不能在同一时间参与多次交易。

<!-- more -->

---

### 121. Best Time to Buy and Sell Stock

Say you have an array for which the *i*th element is the price of a given stock on day *i*.

If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

**Example 1:**

```
Input: [7, 1, 5, 3, 6, 4]
Output: 5

max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)
```

**Example 2:**

```
Input: [7, 6, 4, 3, 1]
Output: 0

In this case, no transaction is done, i.e. max profit = 0.
```

---

#### 算法：动态规划

**设dp[i]表示在第i个时刻卖出所能获得的最大利润。**为了计算dp[i]，我们只要知道第i个元素前面的最小元素，最小的时候买入，i时刻卖出，就得到了i时刻卖出能获得的最大利润。整个dp[]数组的最大值就是问题的解，由于计算dp[i]只需要知道前i个数中的最小值，所以可以将空间复杂度降到O(1)。

```java
public int maxProfit(int[] prices) {
    int min = Integer.MAX_VALUE;
    int max = 0;
    for (int i = 0; i < prices.length; i++) {
        if (prices[i] < min) min = prices[i];
        int cur = prices[i] - min;
        if (cur > max) max = cur;
    }
    return max;
}
```

---

### 122. Best Time to Buy and Sell Stock II

Say you have an array for which the *i*th element is the price of a given stock on day *i*.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

---

#### 算法1: 波峰浪谷法

我们将股票画成一个曲线图，找出曲线中所有的极大值peak和极小值valley，累加所有极大值减去前一个极小值的差值，才能获得最大利润，如果跳过一个极大值和一个极小值，就不能获得最大利润。

即 $MaxProfit = \sum_{i}(height(peak_i) - height(valley_i)) $

证明：数组[7, 1, 5, 3, 6, 4]的曲线图如下所示：

![Profit Graph](https://leetcode.com/media/original_images/122_maxprofit_1.PNG)

最大利润一定等于（peaki-valleyi) + (peakj - valleyj)，如果我们跳过peaki和valleyj，直接用peakj-valleyi来获得最大差值，则最后得到的利润将比之前的方法少(peaki-valleyj).

```java
public int maxProfit(int[] prices) {
    if (prices == null || prices.length < 2) return 0;
    int i = 0;
    int valley = prices[0];
    int peak = prices[0];
    int maxprofit = 0;
    while (i < prices.length - 1) {
        while (i < prices.length - 1 && prices[i] >= prices[i+1]) i++;
        valley = prices[i];
        while (i < prices.length - 1 && prices[i] <= prices[i+1]) i++;
        peak = prices[i];
        maxprofit += peak - valley;
    }
    return maxprofit;
}
```

时间复杂度：O(n);

空间复杂度：O(1);

#### 算法2: 直接累加所有有收益的股票对

从前到后遍历数组，如果后一个数比前一个数大说明有收益，则累加收益，累加所有收益就可以得到最大收益。

```java
public int maxProfit(int[] prices) {
    if (prices == null || prices.length < 2) return 0;
    int maxprofit = 0;
    for (int i = 1; i < prices.length; i++) {
        if (prices[i] > prices[i-1]) 
            maxprofit += prices[i] - prices[i-1];
    }
    return maxprofit;
}
```

时间复杂度：O(n)

空间复杂度：O(1)

---

### 123. Best Time to Buy and Sell Stock III

Say you have an array for which the *i*th element is the price of a given stock on day *i*.

Design an algorithm to find the maximum profit. You may complete at most *two* transactions.

**Note:**
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

---

#### 算法：动态规划

由于我们可以进行两次买卖操作，所以我们设置四个变量：

- buy1，buy2表示第一次买入和第二次买入时**身上剩下的钱**，我们设置初始状态时身上的钱数是0，所以buy1第一次买入的时候身上的钱数一定是负数，第二次买入的时候，由于可能第一次的股票卖出后挣了钱，所以buy2可能是正数，可能是负数。
- profit1，profit2表示我么卖出两次买入股票时**挣的钱**，那么最后的答案就是第二次卖出时挣的钱。

对于这四个变量我们在遍历时都取最大值。

```java
public int maxProfit(int[] prices) {
    int buy1 = Integer.MIN_VALUE, buy2 = Integer.MIN_VALUE;
    int profit1 = 0, profit2 = 0;
    for (int price : prices) {
        profit2 = Math.max(profit2, buy2 + price);//第二次卖出获得的利润
        buy2 = Math.max(buy2, profit1 - price);//第二次买入剩下的钱是第一次的利润减当前股票价
        profit1 = Math.max(profit1, buy1 + price);//卖出第一次买入的股票挣的钱
        buy1 = Math.max(buy1, -price);//第一次买入时剩下的钱是负数。
    }
    return profit2;
}
```

---

### 188. Best Time to Buy and Sell Stock IV

Say you have an array for which the *i*th element is the price of a given stock on day *i*.

Design an algorithm to find the maximum profit. You may complete at most **k** transactions.

**Note:**
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

**Credits:**
Special thanks to [@Freezen](https://oj.leetcode.com/discuss/user/Freezen) for adding this problem and creating all test cases.

---

#### 算法：动态规划

**设dp(i, j)表示到达第j天时，最多可以进行i次交易，可获得的最大利润。**

- 如果k >= (len/2) 即可以进行买卖的次数大于股票的数量，则直接返回122题的结果，即将所有股票能获得的利润累加。

- 如果k < (len/2) 即进行买卖的次数小于股票的数量时，我们引入变量tmpMax, 表示？，设开始时手中的钱数为0，tmpMax的初值为第一天买入股票后手中剩下的钱，则dp(i, j)应该等于下面两者中较大的那一个：

  1. 到达第j天时手里的利润，即第j天时不进行买出操作。
  2. 如果在第j天时进行买出操作，能拿到的利润。

  tmpMax应该等于下面两者中较大的那个(注意tmpMax有可能时负数)：

  1. 在第j-1天时进行买入操作，手里剩下的钱。
  2. 什么都不做手里剩下的钱。

时间复杂度：O(k*n);

空间复杂度：O(k*n);

```java
public int maxProfit(int k, int[] prices) {
    if (k == 0 || prices.length < 2) return 0;
    int len = prices.length;
    
    //如果可以进行买卖的次数大于股票的数量，则直接返回122题的结果。
    if (k >= (len/2)) return helper(prices);
    
    int[][] dp = new int[k+1][len];
    for (int i = 1; i <= k; i++) { //i表示最大可以进行的交易次数
        int tmpMax = -prices[0]; //tmpMax表示手里剩下的钱
        for (int j = 1; j < len; j++) { //j表示到达第j天
            dp[i][j] = Math.max(dp[i][j-1], prices[j]+tmpMax);
            tmpMax = Math.max(tmpMax, dp[i-1][j-1] - prices[j]);
        }
    }
    return dp[k][len-1];
}
public int helper(int[] prices) {
    int len = prices.length;
    int maxprofit = 0;
    for (int i = 1; i < len; i++) {
        if (prices[i] > prices[i-1]) {
            maxprofit += prices[i] - prices[i-1];
        }
    }
    return maxprofit;
}
```

#### *算法2: 动态规划

如果买卖的数量大于股票的数量，则直接计算出所有股票能获得的最大利润：定义两个变量ktHold0，ktHold1。ktHold0表示**卖**出股票后手中剩下的钱，ktHold1表示**买**入股票后手中剩下的钱。ktHold0的初值为0，即正道的钱。ktHold1的初值为Integer.MIN_VALUE，即初始情况下手中的钱。

对于每天的price，ktHold0（卖）的值为以下两种情况的最大值：

- 卖出股票后手中的钱：ktHold1 + price
- 这次不卖手中的钱：ktHold0

对于每天的price，ktHold1（买）的值为以下两种情况的最大值：

- 买入股票剩下的钱：prevktHold0 - price
- 这次不买原来手中的钱：ktHold1

如果买卖的次数小于股票的数量，则我们定义两个数组，hold0[]表示每次买出后剩下的钱，hold1[]表示每次买入后剩下的钱。

**这种算法同样可以用到前面几道题，设k的值为2就是123题的解**。

```java
public int maxProfit(int k, int[] prices) {
    if (prices == null || prices.length < 2)
        return 0;
    
    if (k >= prices.length/2) {
        int ktHold0 = 0;
        int ktHold1 = Integer.MIN_VALUE;
        for (int price : prices) {
            int prevktHold0 = ktHold0;
            ktHold0 = Math.max(ktHold0, ktHold1 + price); // sell
            ktHold1 = Math.max(ktHold1, prevktHold0 - price); // buy
        }
        return ktHold0;
    }

    int[] hold0 = new int[k + 1];
    int[] hold1 = new int[k + 1];
    Arrays.fill(hold1, Integer.MIN_VALUE);
    for (int price : prices) {
        for (int j = k; j > 0; j--) {
            hold0[j] = Math.max(hold0[j], hold1[j] + price); // sell
            hold1[j] = Math.max(hold1[j], hold0[j - 1] - price); // buy
        }
    }
    return hold0[k];
}
```

---

### 309. Best Time to Buy and Sell Stock with Cooldown

Say you have an array for which the *i*th element is the price of a given stock on day *i*.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

- You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
- After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

**Example:**

```
prices = [1, 2, 3, 0, 2]
maxProfit = 3
transactions = [buy, sell, cooldown, buy, sell]
```

**Credits:**
Special thanks to [@dietpepsi](https://leetcode.com/discuss/user/dietpepsi) for adding this problem and creating all test cases.

---

#### 算法：动态规划

我们设置三个数组分别表示用户处于三种状态买，买，冻结期时所获得的最大收益：

**buy[i]表示第i天之前最后一个操作是买，获得的最大收益。**

**sell[i]表示第i天之前最后一个操作是卖，获得的最大收益。**

**rest[i]表示第i天之前最后一个操作是冻结，获得的最大收益。**

则我们可以得到以下递推公式：

```
buy[i] = max(rest[i-1] - price, buy[i-1])
sell[i] = max(buy[i-1] + price, sell[i-1])
rest[i] = max(sell[i-1], buy[i-1], rest[i-1])
```

由于处于冻结期时手里的钱数肯定要大于刚买完股票时手里的钱数，所以rest[i] >= buy[i] 所以上面的rest[i]可以简化为rest[i] = max(sell[i-1], rest[i-1])。进一步观察可以发现rest[i] <= sell[i]，所以rest[i] = sell[i-1]，上面的方程可以简化为两个：

```
buy[i] = max(sell[i-2] - price, buy[i-1])
sell[i] = max(buy[i-1] + price, sell[i-1])
```

由于处于第i天的状态只依赖于i-1天和i-2天的状态，所以我们可以把空间复杂度降到O(1);

```java
public int maxProfit(int[] prices) {
    int sell = 0, prev_sell = 0, buy = Integer.MIN_VALUE, prev_buy;
    for (int price : prices) {
        prev_buy = buy;
        buy = Math.max(prev_sell - price, prev_buy);
        prev_sell = sell;
        sell = Math.max(prev_buy + price, prev_sell);
    }
    return sell;
}
```

