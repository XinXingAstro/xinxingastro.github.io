---
title: Leetcode91. Decode Ways
tags:
  - 动态规划
comments: true
mathjax: false
date: 2018-04-11 15:08:39
categories: Leetcode题解
---

给出一串数字，求解码的方法数。

<!-- more -->

### 91. Decode Ways

A message containing letters from `A-Z` is being encoded to numbers using the following mapping:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Given an encoded message containing digits, determine the total number of ways to decode it.

For example,
Given encoded message `"12"`, it could be decoded as `"AB"` (1 2) or `"L"` (12).

The number of ways decoding `"12"` is 2.



#### 算法：Fibonacci数列

**设 f[i] 表示长度为 i 的字符串解码的方法数。**则如果字符串长度为1，解码的方式最多有1种，即 f(1) = 1，如果字符串的长度为2，则解码的方式最多有两种，即 f(2) = 2，对于长度为n的字符串，如果第一次取一个字符，此时解法数等于后面n-1个字符的解法数，如果第一次取两个字符，则此时的解法数等于后面n-2个字符的解法数，即 f(n) = f(n-1) + f(n-2)。

**注意：由于可能出现不能解码的情况，所以计算每一个f[i]的时候都要判断f(i-1)和f(i-2)是否存在。**

我们用两个变量cur, prev分别指向当前字符和前面一个字符：

1. 如果当前字符为‘0’，由于单个字符‘0’不对应任何字母，所以‘0’必须和前面字符组在一起，由于只有26个字母，所以只可能是"10", "20"，如果'0'前面出现其他字符则该字符串无法解码，直接返回0，如果前面是'1'或'2'则 `f(i) = f(i-2)`。
2. 如果当前字符不等于'0'则，则至少可以取当前一个字符解码，即`f(i) = f(i-1)`，如果当前字符和前面一个字符组在一起后在10～26之间，则可以取两个字符解码，即`f(i) = f(i-2)`。

时间复杂度：O(n)；空间复杂度：O(n)；

```java
public int numDecodings(String s) {
    if(s == null || s.length() == 0) return 0;
    int len = s.length();
    int[] dp = new int[len+1];
    dp[0] = 1;
    char cur = '#', prev = '#';
    for(int i = 0; i < len; i++) {
        cur = s.charAt(i);
        if(cur == '0') {
            if(!(prev == '1' || prev == '2')) return 0;
            dp[i+1] = dp[i-1];
        } else {
            dp[i+1] = dp[i];
            if(prev == '1' || prev == '2' && cur >= '1' && cur <= '6') 
                dp[i+1] += dp[i-1];
        }
        prev = cur;
    }
    return dp[len];
}
```

