---
title: Leetcode3. Longest Substring Without Repeating Characters
tags:
  - 字符串
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-12 18:04:34
categories: Leetcode题解
---

找到字符串中最长不含重复字符的子串。

剑指Offer面试题48: 最长不含重复字符的子字符串。

<!-- more -->

---

### 3. Longest Substring Without Repeating Characters

Given a string, find the length of the **longest substring** without repeating characters.

**Examples:**

Given `"abcabcbb"`, the answer is `"abc"`, which the length is 3.

Given `"bbbbb"`, the answer is `"b"`, with the length of 1.

Given `"pwwkew"`, the answer is `"wke"`, with the length of 3. Note that the answer must be a **substring**, `"pwke"` is a *subsequence*and not a substring.

---

#### 算法1: 滑动窗口法

使用滑动窗口算法，用count记录子串中重复字符的个数，每当count==0的时候，记录子串的长度，找到所有子串中最长的。

```java
public int lengthOfLongestSubstring(String s) {
    if (s == null || s.length() == 0) 
        return 0;
    int[] map = new int[256];
    char[] sChar = s.toCharArray();
    int len = 0;
    int count = 0;
    int start = 0;
    for (int end = 0; end < sChar.length; end++) {
        //如果value值大于1说明出现重复字符
        if (++map[sChar[end]] > 1)
            count++;
        
        //在子串中有重复字符的情况下移动start指针，直到子串中没有重复字符
        while (count > 0) {
            if (map[sChar[start++]]-- > 1)
                count--;
        }
        //记录子串的最大长度
        len = Math.max(len, end - start + 1);
    }
    return len;
}
```

Leetcode运行时间：49ms

#### 算法2: 暴力搜索法

设置两个指针，第一个指针每向前移动一个位置，就跟前面的不含重复字符的子串的每个字符比较，看是否出现重复，在此过程中记录子串的最大长度。

```java
public int lengthOfLongestSubstring(String s) {
    if (s.length() == 0) {
        return 0;
    }
    int maxLen = 0; //maxLen表示最长不含重复字符的子串
    int len = 1; //len表示目前不含重复字符的子串的长度
    int checkIndex = 0; //当前不含重复字符子串的开始位置
    char[] chars = s.toCharArray();
    for (int i = 1; i < chars.length; i++) {
        len++;
        for (int j = checkIndex; j < i; j++) {
            if (chars[i] == chars[j]) {
                if (maxLen < len - 1) {
                    maxLen = len - 1;
                }
                checkIndex = j + 1;
                len = i - j;
                break;
            }
        }
    }
    return Math.max(maxLen, len);
}
```

Leetcode运行时间：40ms

#### 算法3: 动态规划

设函数f(i)表示以第i个字符结尾的不包含重复字符的子串的最长长度，d表示第i个字符和它上次出现在字符串中位置的距离，则有：

1）若第i个字符之前没有出现过，则`f(i) = f(i-1) + 1`

2）若第i个字符之前出现过

- 若`d <= f(i-1)`则说明第i个字符在前一个最长不重复子串中出现，此时`f(i) = d`
- 若`d > f(i-1)`则说明第i个字符出现在前一个最长不重复子串之前，此时`f(i) = f(i-1) + 1`

```java
public int lengthOfLongestSubstring(String s) {
    if (s == null || s.length() == 0)
        return 0;
    int curLength = 0;
    int maxLength = 0;
    int[] position = new int[256];
    for (int i = 0; i < 256; i++)
        position[i] = -1;
    char[] sChar = s.toCharArray();
    for (int i = 0; i < sChar.length; i++) {
        int prevIndex = position[sChar[i]];
        if (prevIndex < 0 || i - prevIndex > curLength)
            curLength++;
        else 
            curLength = i - prevIndex;
        position[sChar[i]] = i;
        if (curLength > maxLength)
            maxLength = curLength;
    }
    return maxLength;
}
```

Leetcode运行时间：39ms