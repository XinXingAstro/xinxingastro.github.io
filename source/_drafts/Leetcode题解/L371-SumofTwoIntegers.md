---
title: Leetcode371. Sum of Two Integers
tags:
  - 位操作
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-30 18:05:26
categories: Leetcode题解
---

不用`+`.`-`做两数加法。

相关题目：剑指Offer面试题65: 不用加减惩处做加法。

<!-- more -->

---

### 371. Sum of Two Integers

Calculate the sum of two integers *a* and *b*, but you are **not allowed** to use the operator `+` and `-`.

**Example:**
Given *a* = 1 and *b* = 2, return 3.

**Credits:**
Special thanks to [@fujiaozhu](https://discuss.leetcode.com/user/fujiaozhu) for adding this problem and creating all test cases.

---

#### 算法：位操作

假设我们输入的两个数为a，b，那么`a ^ b`就相当于`a + b`不带进位，`a & b`就得到了需要进位的位，所以要使用位操作做两数加法分以下几步:

1）我们先记录需要进位的位，将结果左移一位然后赋值给carry：`carry = (a & b) << 1;`

2）然后我们得到a和b相加不带进位的结果赋值给a：`a = (a ^ b);`

3）然后我们将carry赋值给b，重复上述步骤，知道b=0为止。

```java
public int getSum(int a, int b) {
    int carry;
    while(b != 0) {
        carry = (a & b) << 1;
        a = (a ^ b);
        b = carry;
    }
    return a;
}
```

递归实现：

```java
public int getSum(int a, int b) {
    return b == 0 ? a : getSum((a ^ b), (a & b) << 1);
}
```

#### 扩展：不用加减乘除做减法

算法跟上面的算法一样，我们只需要将一个操作数变为它的负数就相当于做了减法，负数在内存中是以补码形式存储，得到一个数的补码要进行取反加一操作：`A = (~a)+ 1;`A中存的就是a的补码。