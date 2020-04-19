---
title: Leetcode394. Decode String
tags:
  - 字符串
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-13 11:01:02
categories: Leetcode题解
---

给出一个字符串形如：3[a2[b]]，返回：abbabbabb。

<!-- more -->

---

### 394. Decode String

Given an encoded string, return it's decoded string.

The encoding rule is: `k[encoded_string]`, where the *encoded_string* inside the square brackets is being repeated exactly *k* times. Note that *k* is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, *k*. For example, there won't be input like `3a` or `2[4]`.

**Examples:**

```
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```

---

#### 算法：遍历

该题的难点是解题逻辑的建立，我们新建一个numStack用来存储当前字符串的重复次数，新建一个strStack用来存储嵌套在外部的字符串，如果不存在嵌套，strStack中一直都是空字符串，新建一个StringBuilder res用来在结果字符串后面添加字符（可以避免每次添加字符都新建一个字符串），算法分以下几种情况：

1）如果当前位置为字符，则将该字符加到res后。

2）如果当前字符的数字，则计算出该数组的具体值。

3）如果当前字符为'['，则说明即将出现一个新的字符串，那么我们将当前字符串先放到栈中，将res指向新的StringBuilder。

4）如果当前字符为']'，则说明当前的字符串已经结束，那么我们我们要将外面的字符串先取出，然后在它后面重复numStack.peek()次当前字符串。如a2[b]，遇到']'时我们先取出a，然后在a后面append2个b。

```java
public String decodeString(String s) {
    LinkedList<Integer> numStack = new LinkedList<>();
    LinkedList<StringBuilder> strStack = new LinkedList<>();
    StringBuilder res = new StringBuilder();
    char[] chrs = s.toCharArray();
    for (int i = 0; i < chrs.length; i++) {
        if (Character.isDigit(chrs[i])) {
            int count = 0;
            while (Character.isDigit(chrs[i])) {
                count = count * 10 + (chrs[i++]-'0');
            }
            numStack.push(count);
            i--;
        } else if (chrs[i] == '[') {
            strStack.push(res);
            res = new StringBuilder();
        } else if (chrs[i] == ']') {
            StringBuilder tmp = strStack.pop();
            int num = numStack.pop();
            for (int j = 0; j < num; j++) {
                tmp.append(res);
            }
            res = tmp;
        } else {
            res.append(chrs[i]);
        }
    }
    return res.toString();
}
```

时间复杂度：O(n)

空间复杂度：O(n)