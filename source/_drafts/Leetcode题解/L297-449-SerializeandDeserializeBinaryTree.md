---
title: Leetcode297/449. Serialize and Deserialize Binary Tree
tags:
  - 二叉树
comments: true
date: 2018-04-02 15:02:26
categories: Leetcode题解
---
**序列化**和**反序列化**二叉树。

<!-- more -->

### 297. Serialize and Deserialize Binary Tree

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

For example, you may serialize the following tree

```
    1
   / \
  2   3
     / \
    4   5
```

as `"[1,2,3,null,null,4,5]"`, just the same as [how LeetCode OJ serializes a binary tree](https://leetcode.com/faq/#binary-tree). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

 

**Note:** Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

**Credits:**
Special thanks to [@Louis1992](https://leetcode.com/discuss/user/Louis1992) for adding this problem and creating all test cases.



算法：

序列化：前序遍历二叉树，遇到null节点以字符串“X”替代，每个节点间用","分割。

反序列化：根据前序遍历序列递归新建二叉树。

```java
private static final String NULL = "X";
private static final String SPLITOR = ",";

// Encodes a tree to a single string.
public String serialize(TreeNode root) {
    StringBuilder sb = new StringBuilder();
    preorderWalk(root, sb);
    return sb.toString();
}
private void preorderWalk(TreeNode root, StringBuilder sb) {
    if (root == null) {
        sb.append(NULL).append(SPLITOR);
        return;
    }
    sb.append(root.val).append(SPLITOR);
    preorderWalk(root.left, sb);
    preorderWalk(root.right, sb);
}

// Decodes your encoded data to tree.
public TreeNode deserialize(String data) {
    Queue<String> queue = new LinkedList<>();
    queue.addAll(Arrays.asList(data.split(SPLITOR)));
    return build(queue);
}
private TreeNode build(Queue<String> queue) {
    String p = queue.poll();
    if (p.equals(NULL)) return null;
    TreeNode root = new TreeNode(Integer.valueOf(p));
    root.left = build(queue);
    root.right = build(queue);
    return root;
}
```

**注意：几种变量之间的转换技巧：**

`queue.addAll(Arrays.asList(data.split(SPLITOR))); //直接将可分割的String转换成List`

`TreeNode root = new TreeNode(Integer.valueOf(p)); //将String类型变量转换成Integer`



### 449. Serialize and Deserialize BST

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment. 

Design an algorithm to serialize and deserialize a **binary search tree**. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary search tree can be serialized to a string and this string can be deserialized to the original tree structure.

**The encoded string should be as compact as possible.**

**Note:** Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

算法同上。