---
title: Leetcode87. Scramble String
tags:
  - 动态规划
comments: true
mathjax: false
date: 2018-04-11 10:23:49
categories: Leetcode题解
---

给出两个相同长度的字符串 s1 和 s2，判断 s1 是否是 s2 的 scramble string。

<!-- more -->

### 87. Scramble String

Given a string *s1*, we may represent it as a binary tree by partitioning it to two non-empty substrings recursively.

Below is one possible representation of *s1* = `"great"`:

```
    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t
```

To scramble the string, we may choose any non-leaf node and swap its two children.

For example, if we choose the node `"gr"` and swap its two children, it produces a scrambled string `"rgeat"`.

```
    rgeat
   /    \
  rg    eat
 / \    /  \
r   g  e   at
           / \
          a   t
```

We say that `"rgeat"` is a scrambled string of `"great"`.

Similarly, if we continue to swap the children of nodes `"eat"` and `"at"`, it produces a scrambled string `"rgtae"`.

```
    rgtae
   /    \
  rg    tae
 / \    /  \
r   g  ta  e
       / \
      t   a
```

We say that `"rgtae"` is a scrambled string of `"great"`.

Given two strings *s1* and *s2* of the same length, determine if *s2* is a scrambled string of *s1*.

#### 算法：递归

如果两字符串完全相同，则返回 true。

我们用 letters 数组统计两个数组中各字符出现的次数，如果两个字符串中出现不同字符，则直接返回 false。

然后我们递归的对两个字符串进行拆分，如果各个部分都相同，则返回 true，如果不同则返回false。

```java
public boolean isScramble(String s1, String s2) {
    if (s1.equals(s2)) return true; 

    int[] letters = new int[26];
    for (int i=0; i<s1.length(); i++) {
        letters[s1.charAt(i)-'a']++;
        letters[s2.charAt(i)-'a']--;
    }
    for (int i=0; i<26; i++) if (letters[i]!=0) return false;

    for (int i=1; i<s1.length(); i++) {
        if (isScramble(s1.substring(0,i), s2.substring(0,i)) 
         && isScramble(s1.substring(i), s2.substring(i))) return true;
        if (isScramble(s1.substring(0,i), s2.substring(s2.length()-i)) 
         && isScramble(s1.substring(i), s2.substring(0,s2.length()-i))) return true;
    }
    return false;
}
```



