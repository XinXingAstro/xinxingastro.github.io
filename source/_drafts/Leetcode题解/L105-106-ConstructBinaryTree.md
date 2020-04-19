---
title: Leetcode105/106. Construct Binary Tree
tags:
  - 二叉树
comments: true
date: 2018-03-26 02:25:22
categories: Leetcode题解
---
根据前序遍历序列和中序遍历序列，或者后序遍历序列和中序遍历序列重建二叉树。

<!-- more -->

### 105. Construct Binary Tree from Preorder and Inorder Traversal

Given preorder and inorder traversal of a tree, construct the binary tree.

**Note:**
You may assume that duplicates do not exist in the tree.

For example, given

```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
```



使用前序遍历序列和中序遍历序列重建二叉树步骤：

I. 在中序遍历序列中找到根节点下标

II. 利用该下标和中序遍历开始节点下标，计算出二叉树左子树长度

III. 新建根节点，根据左子树长度递归新建根节点左子树和右子树

```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    if (preorder == null || inorder == null) {
        return null;
    }
    HashMap<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < inorder.length; i++) {
        map.put(inorder[i], i);
    }
    return reBuild(preorder, inorder, 0, preorder.length-1, 0, map);
}
public TreeNode reBuild(int[] pre, int[] in, int preStart, int preEnd, int inStart, HashMap<Integer, Integer> map) {
    if (preStart > preEnd) {
        return null;
    }
    int inRoot = map.get(pre[preStart]);
    int lengthOfLeft = inRoot - inStart;
    TreeNode root = new TreeNode(pre[preStart]);
    root.left = reBuild(pre, in, preStart+1, preStart+lengthOfLeft, inRoot-lengthOfLeft, map);
    root.right = reBuild(pre, in, preStart+lengthOfLeft+1, preEnd, inRoot+1, map);
    return root;
}
```



### 106. Construct Binary Tree from Inorder and Postorder Traversal

Given inorder and postorder traversal of a tree, construct the binary tree.

**Note:**
You may assume that duplicates do not exist in the tree.

For example, given

```
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
```



```java
public TreeNode buildTree02(int[] inorder, int[] postorder) {
    if (inorder == null || postorder == null) {
        return null;
    }
    HashMap<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < inorder.length; i++) {
        map.put(inorder[i], i);
    }
    return reBuild02(inorder, postorder, 0, postorder.length - 1, 0, map);
}
public TreeNode reBuild02(int[] in, int[] pos, int posStart, int posEnd, int inStart, HashMap<Integer, Integer> map) {
    if (posStart > posEnd) {
        return null;
    }
    int inRoot = map.get(pos[posEnd]);
    int lengthOfLeft = inRoot - inStart;
    TreeNode root = new TreeNode(pos[posEnd]);
    root.left = reBuild02(in, pos, posStart, posStart + lengthOfLeft - 1, inStart, map);
    root.right = reBuild02(in, pos, posStart + lengthOfLeft, posEnd - 1, inRoot + 1, map);
    return root;
}
```

