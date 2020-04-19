---
title: Leetcode76. Minimum Window Substring
tags:
  - 字符串
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-12 16:51:02
categories: Leetcode题解
---

给出两个字符串S和T，找出S中包含T全部字符的最小长度子串。

<!-- more -->

---

### 76. Minimum Window Substring

Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

**Example:**

```
Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
```

**Note:**

- If there is no such window in S that covers all characters in T, return the empty string `""`.
- If there is such window, you are guaranteed that there will always be only one unique minimum window in S.

---

#### 算法：滑动窗口法

用字符串的滑动窗口算法，当count减少到0的时候说明此时窗口内已经包含了T中所有的字符，我们只要记录每次count==0的时候窗口的长度，然后找出最小长度，截取此时S的子串就是解。

```java
public String minWindow(String s, String t) {
    int[] map = new int[256];
    String res = "";
    if (s == null || t.length() > s.length()) 
        return res;
    char[] sChar = s.toCharArray();
    for (char c : t.toCharArray()) {
        map[c]++;
    }
    int min = sChar.length;
    int count = t.length();
    int start = 0;
    for (int end = 0; end < sChar.length; end++) {
        if (map[sChar[end]]-- > 0)
            count--;
        while (count == 0) {
            if (end - start < min) {
                min = end - start;
                res = s.substring(start, end + 1);
            }
            if (++map[sChar[start++]] > 0)
                count++;
        }
    }
    return res;
}
```

