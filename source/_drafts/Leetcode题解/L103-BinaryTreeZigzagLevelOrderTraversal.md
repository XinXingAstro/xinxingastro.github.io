---
title: Leetcode103. Binary Tree Zigzag Level Order Traversal
tags:
  - 二叉树
comments: true
date: 2018-03-26 01:16:00
categories: Leetcode题解
---
之字型打印二叉树，二叉树层次遍历的扩展，剑指Offer32题题型三。

<!-- more -->

### 103. Binary Tree Zigzag Level Order Traversal

Given a binary tree, return the *zigzag level order* traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:
Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its zigzag level order traversal as:

```
[
  [3],
  [20,9],
  [15,7]
]
```

算法：用两个栈，一个栈在树左边从左到右压入每一层的节点，一个栈在树右边从右向左压入树每一层节点，具体操作如下：

| 操作       | rightStack | leftStack |
| ---------- | ---------- | --------- |
|            | 3          |           |
| 访问节点3  |            | 20,9      |
| 访问节点20 | 15,7       | 9         |
| 访问节点9  | 15,7       |           |
| 访问节点15 | 7          |           |
| 访问节点7  |            |           |

```java
public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
    if (root == null) {
        return new ArrayList<>();
    }
    List<List<Integer>> res = new ArrayList<>();
    Stack<TreeNode> rightStack = new Stack<>();
    Stack<TreeNode> leftStack = new Stack<>();
    rightStack.push(root);
    while (!rightStack.isEmpty() || !leftStack.isEmpty()) {
        TreeNode p;
        List<Integer> list = new ArrayList<>();
        while (!rightStack.isEmpty()) {
            p = rightStack.pop();
            list.add(p.val);
            if (p.left != null)
                leftStack.push(p.left);
            if (p.right != null)
                leftStack.push(p.right);
        }
        if (!list.isEmpty()) {
            res.add(list);
            list = new ArrayList<>();
        }
        while (!leftStack.isEmpty()) {
            p = leftStack.pop();
            list.add(p.val);
            if (p.right != null)
                rightStack.push(p.right);
            if (p.left != null)
                rightStack.push(p.left);
        }
        if (!list.isEmpty())
            res.add(list);
    }
    return res;
}
```

