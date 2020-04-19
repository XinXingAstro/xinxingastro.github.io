---
title: Leetcode856. Score of Parentheses
tags:
  - 分治法
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-25 16:38:07
categories: Leetcode题解
---

给出一串匹配的括号，按照题目规则计算这串括号的值。

<!-- more -->

---

### 856. Score of Parentheses

Given a balanced parentheses string `S`, compute the score of the string based on the following rule:

- `()` has score 1
- `AB` has score `A + B`, where A and B are balanced parentheses strings.
- `(A)` has score `2 * A`, where A is a balanced parentheses string.

 

**Example 1:**

```
Input: "()"
Output: 1
```

**Example 2:**

```
Input: "(())"
Output: 2
```

**Example 3:**

```
Input: "()()"
Output: 2
```

**Example 4:**

```
Input: "(()(()))"
Output: 6
```

 

**Note:**

1. `S` is a balanced parentheses string, containing only `(` and `)`.
2. `2 <= S.length <= 50`

---

#### 算法1: 递归法

我们可以使用递归的思想来解决这个问题：

1）如果括号只有1层，结果直接加1即可

2）如果括号有多层，结果应该是内层括号的值乘2，内层括号的值我们可以递归得到。

```java
public int scoreOfParentheses(String S) {
    return helper(S, 0, S.length() - 1);
}
public int helper(String S, int start, int end) {
    int count = 0, res = 0;
    for (int i = start; i <= end; i++) {
        count += S.charAt(i) == '(' ? 1 : -1;
        if (count == 0) {
            if (i - start == 1) 
                res++;
            else 
                res += 2 * helper(S, start + 1, i - 1);
            start = i + 1;
        }
    }
    return res;
}
```

时间复杂度：O(n)

空间复杂度：O(n)

#### 算法2: 计算嵌套层数

通过观察题目我们不能发现，一个括号串的值是跟括号嵌套层数相关的，一层括号的值为1，二层的值为2，三层的值为4，四层的值为8......可知括号表达式的值 = 1 << 嵌套层数。

例如`(()(()))`这个括号的值为`2 * (1 + 2) = 2 + 4 = 6 `。

```java
public int scoreOfParentheses(String S) {
    int count = 0, res = 0;
    for (int i = 0; i < S.length(); i++) {
        if (S.charAt(i) == '(') {
            count++;
        } else {
            count--;
            if (S.charAt(i - 1) == '(')
                res += 1 << count;
        }
    }
    return res;
}
```

时间复杂度：O(n)

空间复杂度：O(1)