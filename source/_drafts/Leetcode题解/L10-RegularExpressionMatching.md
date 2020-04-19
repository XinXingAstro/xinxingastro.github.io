---
title: Leetcode10/44. Regular Expression Matching / Wildcard Matching
tags:
  - 动态规划
comments: true
mathjax: true
date: 2018-04-05 17:24:32
categories: Leetcode题解
---

带有'.'和'*'的字符串模式匹配。

<!-- more -->

### 10. Regular Expression Matching

Implement regular expression matching with support for `'.'` and `'*'`.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
```

#### 算法1：递归

如果 pattern 中当前字符的后一个字符不是 '*'，我们递归的从左到右检查 text 和 pattern 。

如果 pattern 中当前字符的后一个字符是 '*'，我们有两种选择：

1. 忽略 pattern 中当前字符和 '\*'， pattern 向后移动两位，然后继续向后检查。即 '*' 代表其前面字符出现0次，
2. pattern 中的 '*' 代表其前面字符出现多次，pattern 不动 text 向后移动。

设 T 表示 text 的长度，P 不是 pattern 的长度。

时间复杂度：$\sum_{i = 0}^{T}\sum_{j = 0}^{P/2}{i + j \choose i}O(T + P - i - 2j)$ => $O((T + P)2^{T + \frac{P}{2}})$

空间复杂度：$O(T^2 + P^2)$

```java
public boolean isMatch(String text, String pattern) {
    if (pattern.isEmpty()) return text.isEmpty();
    boolean first_match = (!text.isEmpty() && (pattern.charAt(0) == text.charAt(0) || pattern.charAt(0) == '.'));

    if (pattern.length() >= 2 && pattern.charAt(1) == '*'){
        return (isMatch(text, pattern.substring(2)) || (first_match && isMatch(text.substring(1), pattern)));
    } else {
        return first_match && isMatch(text.substring(1), pattern.substring(1));
    }
}
```



#### 算法2: 动态规划

**设 dp[i, j] 表示 text[i] 和 pattern[j] 是否匹配。**

时间复杂度：O(TP)

空间复杂度：O(TP)

从前向后检查：

```java
enum Result {
    TRUE, FALSE
}

class Solution {
    Result[][] memo;
        
    public boolean isMatch(String text, String pattern) {
        memo = new Result[text.length() + 1][pattern.length() + 1];
        return dp(0, 0, text, pattern);
    }
    
    public boolean dp(int i, int j, String text, String pattern) {
        if (memo[i][j] != null) {
            return memo[i][j] == Result.TRUE;
        }
        boolean ans;
        if (j == pattern.length()){
            ans = i == text.length();
        } else{
            boolean first_match = (i < text.length() && 
                                   (pattern.charAt(j) == text.charAt(i) ||
                                    pattern.charAt(j) == '.'));

            if (j + 1 < pattern.length() && pattern.charAt(j+1) == '*'){
                ans = (dp(i, j+2, text, pattern) || 
                       first_match && dp(i+1, j, text, pattern));
            } else {
                ans = first_match && dp(i+1, j+1, text, pattern);
            }
        }
        memo[i][j] = ans ? Result.TRUE : Result.FALSE;
        return ans;
    }
}
```

从后向前检查；

```java
class Solution {
    public boolean isMatch(String text, String pattern) {
        boolean[][] dp = new boolean[text.length() + 1][pattern.length() + 1];
        dp[text.length()][pattern.length()] = true;
        
        for (int i = text.length(); i >= 0; i--){
            for (int j = pattern.length() - 1; j >= 0; j--){
                boolean first_match = (i < text.length() && 
                                       (pattern.charAt(j) == text.charAt(i) ||
                                        pattern.charAt(j) == '.'));
                if (j + 1 < pattern.length() && pattern.charAt(j+1) == '*'){
                    dp[i][j] = dp[i][j+2] || first_match && dp[i+1][j];
                } else {
                    dp[i][j] = first_match && dp[i+1][j+1];
                }
            }
        }
        return dp[0][0];
    }
}
```



### 44. Wildcard Matching

Implement wildcard pattern matching with support for `'?'` and `'*'`.

```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "*") → true
isMatch("aa", "a*") → true
isMatch("ab", "?*") → true
isMatch("aab", "c*a*b") → false
```



```java
public boolean isMatch(String str, String pattern) {
    int s = 0, p = 0, match = 0, starIdx = -1;            
    while (s < str.length()){
        // advancing both pointers
        if (p < pattern.length()  && (pattern.charAt(p) == '?' || str.charAt(s) == pattern.charAt(p))){
            s++;
            p++;
        }
        // * found, only advancing pattern pointer
        else if (p < pattern.length() && pattern.charAt(p) == '*'){
            starIdx = p;
            match = s;
            p++;
        }
       // last pattern pointer was *, advancing string pointer
        else if (starIdx != -1){
            p = starIdx + 1;
            match++;
            s = match;
        }
       //current pattern pointer is not star, last patter pointer was not *
      //characters do not match
        else return false;
    }

    //check for remaining characters in pattern
    while (p < pattern.length() && pattern.charAt(p) == '*')
        p++;

    return p == pattern.length();
}
```

