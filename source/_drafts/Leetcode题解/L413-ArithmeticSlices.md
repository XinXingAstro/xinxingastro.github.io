---
title: Leetcode413. Arithmetic Slices
tags:
  - Math
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-05-10 16:46:25
categories: Leetcode题解
---

给出一个数组，找出数组中最少3个数的等差数列。

<!-- more -->

---

### 413. Arithmetic Slices

A sequence of number is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same.

For example, these are arithmetic sequence:

```
1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
```

The following sequence is not arithmetic.

```
1, 1, 2, 5, 7
```

 

A zero-indexed array A consisting of N numbers is given. A slice of that array is any pair of integers (P, Q) such that 0 <= P < Q < N.

A slice (P, Q) of array A is called arithmetic if the sequence:
A[P], A[p + 1], ..., A[Q - 1], A[Q] is arithmetic. In particular, this means that P + 1 < Q.

The function should return the number of arithmetic slices in the array A. 

 

**Example:**

```
A = [1, 2, 3, 4]

return: 3, for 3 arithmetic slices in A: [1, 2, 3], [2, 3, 4] and [1, 2, 3, 4] itself.
```

 

#### 算法：数学方法

该题让找出数组中所有长度大于三的等差数列的个数，我们首先来看等差数列的个数和长度有什么关系：

| 数列          | 长度大于3的等差数列个数 |
| ------------- | ----------------------- |
| 1 2 3         | 1 = 0 + 1               |
| 1 2 3 4       | 3 = 1 + 2               |
| 1 2 3 4 5     | 6 = 3 + 3               |
| 1 2 3 4 5 6   | 10 = 6 + 4              |
| 1 2 3 4 5 6 7 | 15 = 10 + 5             |

可以看到，睡着等差数列的长度增加，可以得到的大于3的等差数列的个数也逐渐增加，增加的幅度是1, 2, 3, … ，所以我们需要遍历数组找出数组中所有等差数列，根据这些等差数列的长度，计算长度大于3的等差子数列的个数。

```java
public int numberOfArithmeticSlices(int[] A) {
    int dis = 0;
    int res = 0;
    for (int i = 2; i < A.length; i++) {
        if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
            dis++;
            res += dis;
        } else dis = 0;
    }
    return res;
}
```

时间复杂的：O(n)

空间复杂度：O(1)



