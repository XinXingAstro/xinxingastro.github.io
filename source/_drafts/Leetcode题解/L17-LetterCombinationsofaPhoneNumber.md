---
title: Leetcode17. Letter Combinations of a Phone Number
tags:
  - 组合
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-08 00:51:31
categories: Leetcode题解
---

输出电话号码能表示出的所有字符串。

<!-- more -->

---

### 17. Letter Combinations of a Phone Number

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![img](http://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

**Example:**

```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**Note:**

Although the above answer is in lexicographical order, your answer could be in any order you want.

---

#### 算法：字符的全组合

这道题的难点是理解题意，设计循环的执行方案。

```java
private String[] digitLetters = {"","","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
public List<String> letterCombinations(String digits) {
    if (digits.length() == 0) return new ArrayList<>();
    List<String> ans = new ArrayList<>();
    ans.add("");
    for (int i = 0; i < digits.length(); i++) {
        ans = combine(ans, digitLetters[digits.charAt(i) - '0']);
    }
    return ans;
}
public List<String> combine(List<String> ans, String str) {
    List<String> res = new ArrayList<>();
    for (String s : ans) {
        for (int i = 0; i < str.length(); i++) {
            res.add(s + str.charAt(i));
        }
    }
    return res;
}
```