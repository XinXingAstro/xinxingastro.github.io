---
title: Leetcode784. Letter Case Permutation
tags:
  - DFS
  - BFS
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-30 01:41:31
categories: Leetcode题解
---

输入一个字符串，输出将字符串中的字符变为大写或小写的所有组合。

<!-- more -->

---

### 784. Letter Case Permutation

Given a string S, we can transform every letter individually to be lowercase or uppercase to create another string.  Return a list of all possible strings we could create.

```
Examples:
Input: S = "a1b2"
Output: ["a1b2", "a1B2", "A1b2", "A1B2"]

Input: S = "3z4"
Output: ["3z4", "3Z4"]

Input: S = "12345"
Output: ["12345"]
```

**Note:**

- `S` will be a string with length at most `12`.
- `S` will consist only of letters or digits.

---

#### 算法1: 深度优先遍历

我们可以用深度优先遍历算法递归遍历字符串中所有字母，在遍历过程中将字母替换成大写和小写，然后存入结果集。

```java
private List<String> res = new ArrayList();
public List<String> letterCasePermutation(String S) {
    helper(S, 0);
    return res;
}
public void DFS(String s, int index) {
    if (s == null || index == s.length()) {
        res.add(s);
        return;
    }

    if (Character.isLetter(s.charAt(index))) {
        char[] chr = s.toCharArray();
        
        chr[index] = Character.toLowerCase(chr[index]);
        helper(String.valueOf(chr), index + 1);
        
        chr[index] = Character.toUpperCase(chr[index]);
        helper(String.valueOf(chr), index + 1);
    } else {
        helper(s, index + 1);
    }
}
```

#### 算法2: 宽度优先遍历

同样可以用宽度优先遍历算法遍历字符串中所有字母，遍历过程中替换字母为大写和小写，注意最后队列中所有的字符串就是结果集。

```java
public List<String> letterCasePermutation1(String S) {
    List<String> res = new ArrayList();
    if (S == null) return res;
    Queue<String> queue = new ArrayDeque();
    queue.offer(S);
    for (int i = 0; i < S.length(); i++) {
        if (Character.isLetter(S.charAt(i))) {
            int size = queue.size();
            for (int j = 0; j < size; j++) {
                char[] chr = queue.poll().toCharArray();

                chr[i] = Character.toLowerCase(chr[i]);
                queue.offer(String.valueOf(chr));

                chr[i] = Character.toUpperCase(chr[i]);
                queue.offer(String.valueOf(chr));
            }
        }
    }
    return new ArrayList(queue);
}
```

