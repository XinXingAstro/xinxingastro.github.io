---
title: Leetcode32. Longest Valid Parentheses
tags:
  - 动态规划
comments: true
mathjax: true
date: 2018-04-05 23:11:09
categories: Leetcode题解
---

找出字符串中匹配圆括号的最大长度。

<!-- more -->

### 32. Longest Valid Parentheses

Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

For `"(()"`, the longest valid parentheses substring is `"()"`, which has length = 2.

Another example is `")()())"`, where the longest valid parentheses substring is `"()()"`, which has length = 4.



#### 算法1: 动态规划

设 **dp[i] 表示以第 i 个元素结尾的最长匹配圆括号串的长度**。数组 dp[] 的所有元素都初始化为0。显然不存在以 '(' 结尾的匹配圆括号串，所有匹配的圆括号串都是以 ')' 结尾的，所以 dp[] 中所有 '(' 的位置都是0，我们只在 ')' 的位置更新dp[]。

由于圆括号肯定是成对出现的，所以我们两个字符两个字符检查字符串：

1. 若 **s[i] = ')' && s[i - 1] = '('**，如 "…...()"  =>

   ​	**dp[i] = dp[i - 2] + 2**

   因为 s[i] 和 s[i - 1] 组成了一对，所以最长匹配圆括号串的长度会增加2。

2. 若 **s[i] = ')' && s[i - 1] = ')'**，如 "……))" =>

   ​	if **s[i - dp[i - 1] - 1] = '('**  then  **dp[i] = dp[i - 1] + 2 + dp[i - dp[i - 1] - 2]**

   因为 如果倒数第二个 ')' 是最长匹配字符串 $sub_s$ 的一部分，要使最后一个 ')' 是更长匹配子串的一部分，则必须在有一个相应的 '(' 在 $sub_s$ 前面。因此，如果 $sub_s$ 前面的一个字符是 '(' ,我们更新 dp[i] 的值为 $sub_s$ 的长度加2，$sub_s$ 的长度就是dp[i - 1]。由于在 $sub_s$ 前面的 '(' 被加入了最长匹配序列，有可能在 '(' 前面还存在匹配序列，所以我们还要加上前面的匹配序列的长度 **dp[i - dp[i - 1] - 2]**，如"…(())($sub_s$)"。

时间复杂度：O(n) ，只遍历字符串一遍，就可以完成 dp[] 数组

空间复杂度：O(n)

Leetcode 19ms

```java
public int longestValidParentheses(String s) {
    int maxans = 0;
    int dp[] = new int[s.length()];
    for (int i = 1; i < s.length(); i++) {
        if (s.charAt(i) == ')') {
            if (s.charAt(i - 1) == '(') {
                dp[i] = (i >= 2 ? dp[i - 2] : 0) + 2;
            } else if (i - dp[i - 1] > 0 && s.charAt(i - dp[i - 1] - 1) == '(') {
                dp[i] = dp[i - 1] + ((i - dp[i - 1]) >= 2 ? dp[i - dp[i - 1] - 2] : 0) + 2;
            }
            maxans = Math.max(maxans, dp[i]);
        }
    }
    return maxans;
}
```



#### 算法2: 使用栈

由于要计算最大匹配括号串的长度，所以**栈中应保存各个字符的下标**。

**I. 初始我们向栈中压入-1**

II. 对于每一个 '(' 我们压入它的下标

III. 对于每一个 ')' ，如果栈非空，我们弹出栈顶元素，用该元素的下标减去目前的(栈弹元素出后)栈顶元素，就是目前匹配括号串的长度。如果在弹出的过程中，栈变为空，我们就压入当前元素的下标。

时间复杂度：O(n)

空间复杂度：O(n)

Leetcode 25ms

```java
public int longestValidParentheses(String s) {
    int maxans = 0;
    Stack<Integer> stack = new Stack<>();
    stack.push(-1);
    for (int i = 0; i < s.length(); i++) {
        if (s.charAt(i) == '(') {
            stack.push(i);
        } else {
            stack.pop();
            if (stack.empty()) {
                stack.push(i);
            } else {
                maxans = Math.max(maxans, i - stack.peek());
            }
        }
    }
    return maxans;
}
```





#### 算法3: 不使用额外空间

我们需要对字符串做两次遍历，第一次从左到右遍历，第二次从右到左遍历，用两个变量 left 和 right 分别记录左括号和右括号的数量：

1. 从左到右遍历字符串，遇到 '(' left 加1，遇到 ')' right 加1。

   当 left == right 时我们计算当前匹配括号串的长度。

   **当 right > left 时我们将 left 和 right 同时设为0**。

2. 从右到左遍历字符串，遇到 '(' left 加1，遇到 ')' right 加1。

   当 left == right 时计算当前匹配括号串的长度。

   **当 left > right 时我们将 left 和 right 同时设为0**。

整个过程中我们记录最大匹配长度。

时间复杂度：O(n)

空间复杂度：O(1)

Leetcode 20ms

```java
public int longestValidParentheses(String s) {
    int left = 0, right = 0, maxlength = 0;
    for (int i = 0; i < s.length(); i++) {
        if (s.charAt(i) == '(') {
            left++;
        } else {
            right++;
        }
        if (left == right) {
            maxlength = Math.max(maxlength, 2 * right);
        } else if (right >= left) {
            left = right = 0;
        }
    }
    left = right = 0;
    for (int i = s.length() - 1; i >= 0; i--) {
        if (s.charAt(i) == '(') {
            left++;
        } else {
            right++;
        }
        if (left == right) {
            maxlength = Math.max(maxlength, 2 * left);
        } else if (left >= right) {
            left = right = 0;
        }
    }
    return maxlength;
}
```



#### 三种算法比较

动态规划算法由于只遍历字符串一遍所以速度最快，栈算法由于有很多栈的压入弹出操作，所以速度最慢，第三种两次遍历算法空间复杂度最好，由于要对字符串遍历两遍，所以速度在前两个算法之间。