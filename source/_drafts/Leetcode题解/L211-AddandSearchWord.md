---
title: Leetcode211. Add and Search Word - Data structure design
tags:
  - 字典树
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-11 17:07:22
categories: Leetcode题解
---

设计一个数据结构可以实现带通配符'.'字的查找功能。

相关题目：Leetcode208. Implement Trie (Prefix Tree)

<!-- more -->

---

### 211. Add and Search Word - Data structure design

Design a data structure that supports the following two operations:

```
void addWord(word)
bool search(word)
```

search(word) can search a literal word or a regular expression string containing only letters `a-z` or `.`. A `.` means it can represent any one letter.

**Example:**

```
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```

**Note:**
You may assume that all words are consist of lowercase letters `a-z`.

---

#### 算法：递归查找

这道题的难点是设计可以查找带通配符的字的功能，为了实现该功能我们必须在字典树中进行搜索，跳过这个通配符，看剩下的字在字典中能否找到，如果字典树中有一个分支可以找到就返回true。

```java
class WordDictionary {
    class TrieNode {
        // R links to node children
        private TrieNode[] links;

        private final int R = 26;

        private boolean isEnd;

        public TrieNode() {
            links = new TrieNode[R];
        }

        public boolean containsKey(char ch) {
            return links[ch -'a'] != null;
        }
        public TrieNode get(char ch) {
            return links[ch -'a'];
        }
        public void put(char ch, TrieNode node) {
            links[ch -'a'] = node;
        }
        public void setEnd() {
            isEnd = true;
        }
        public boolean isEnd() {
            return isEnd;
        }
    }

    private TrieNode root;
    
    /** Initialize your data structure here. */
    public WordDictionary() {
        root = new TrieNode();
    }
    
    /** Adds a word into the data structure. */
    public void addWord(String word) {
        TrieNode node = root;
        for (int i = 0; i < word.length(); i++) {
            char currentChar = word.charAt(i);
            if (!node.containsKey(currentChar)) {
                node.put(currentChar, new TrieNode());
            }
            node = node.get(currentChar);
        }
        node.setEnd();
    }
    
    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    public boolean search(String word) {
        return searchWord(word, 0, root);
    }
    
    public boolean searchWord(String word, int index, TrieNode root) {
        if (index == word.length())
            return root.isEnd();
        
        char cur = word.charAt(index);
        if (cur != '.') {
           return root.links[cur-'a'] != null && searchWord(word, index + 1, root.links[cur - 'a']);
        } else {
            for (int i = 0; i < 26; i++) {
                if (root.links[i] != null && searchWord(word, index + 1, root.links[i]))
                    return true;
            }
            return false;
        }     
    }
}
```

