---
title: Leetcode338. Counting Bits
tags:
  - 动态规划
  - 位操作
comments: true
mathjax: false
date: 2018-04-20 00:42:21
categories: Leetcode题解
---

给出一个数n, 输出0~n这n+1个数，每个数的二进制表示中1的个数。

相关题目：剑指offer面试题15: 二进制中1的个数。

<!-- more -->

### 338. Counting Bits

Given a non negative integer number **num**. For every numbers **i** in the range **0 ≤ i ≤ num** calculate the number of 1's in their binary representation and return them as an array.

**Example:**
For `num = 5` you should return `[0,1,1,2,1,2]`.

**Follow up:**

- It is very easy to come up with a solution with run time **O(n\*sizeof(integer))**. But can you do it in linear time **O(n)** /possibly in a single pass?
- Space complexity should be **O(n)**.
- Can you do it like a boss? Do it without using any builtin function like **__builtin_popcount** in c++ or in any other language.

**Credits:**
Special thanks to [@ syedee ](https://leetcode.com/discuss/user/syedee)for adding this problem and creating all test cases.

### 算法1: 自底向上动态规划

**设f(i)表示数字i的二进制表示中1的个数**，则有

`f(i) = f(i >> 1) + (i & 1)`

时间复杂度：O(n)

空间复杂度：O(1)

Leetcode: 3ms

```java
public int[] countBits(int num) {
    int[] f = new int[num + 1];
    for (int i = 1; i <= num; i++) 
        f[i] = f[i >> 1] + (i & 1);
    return f;
}
```



### 算法2: 减1相与法

> 一个数减1，然后再和原来这个数相与，会将这个数最右边的1置为0。即`(n - 1) & n`会将n的二进制表示中最右边的1置为0.

时间复杂度：O(n)

空间复杂度：O(1)

Leetcode: 5ms

```java
public int[] countBits(int num) {
    int[] res = new int[num + 1];
    for (int i = 0; i <= num; i++) {
        int n = i, count = 0;
        while (n != 0) {
            count++;
            n = (n - 1) & n;
        }
        res[i] = count;
    }
    return res;
}
```

