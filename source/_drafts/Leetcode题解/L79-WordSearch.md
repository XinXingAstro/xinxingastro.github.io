---
title: Leetcode79. Word Search
tags:
  - 矩阵
  - 回溯法
  - DFS
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-09 14:52:10
categories: Leetcode题解
---

在一个二维字母表中查找一条路径，这条路径包含一个字符串的所有字母。

剑指Offer面试题12: 矩阵中的路径。

<!-- more -->

---

### 79. Word Search

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example:**

```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

---

#### 算法：深度优先搜索

如果在字母表中找到第一个字母，则从该位置开始进行深度优先搜索。

注意下面这个算法判断一个字母是否被访问过的算法：`board[i][j]^=256`，由于字母的ASCII码不超过122（A～Z：65～90，a～z：97～122），用字母的ASCII码值和2^8做异或，相当于将变量的第9位置位1，这样就巧妙的避免了使用boolean矩阵来记录是否访问的空间消耗。

```java
public boolean exist(char[][] board, String word) {
    char[] wrd = word.toCharArray();
    for (int i=0; i<board.length; i++) {
        for (int j=0; j<board[i].length; j++) {
            if (wrd[0] == board[i][j] && search(board, wrd, i, j, 0))
                return true;
        }
    }
    return false;
}

private boolean search(char[][] board, char[] wrd, int i, int j, int index) {
    if (index == wrd.length)
        return true;

    if (i>=board.length || i<0 || j>=board[i].length || j<0 || board[i][j] != wrd[index])
        return false;

    board[i][j] ^= 256;
    boolean exist = search(board, wrd, i+1, j, index+1) ||
        search(board, wrd, i-1, j, index+1) ||
        search(board, wrd, i, j+1, index+1) ||
        search(board, wrd, i, j-1, index+1);
    board[i][j] ^= 256;

    return exist;
}
```

