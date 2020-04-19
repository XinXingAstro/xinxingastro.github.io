---
title: Leetcode450. Delete Node in a BST
tags:
  - 二叉树
comments: true
date: 2018-04-03 15:52:17
categories: Leetcode题解
---
在搜索二叉树中删除一个节点。

<!-- more -->

### 450. Delete Node in a BST

Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

Basically, the deletion can be divided into two stages:

1. Search for a node to remove.
2. If the node is found, delete the node.

**Note:** Time complexity should be O(height of tree).

**Example:**

```
root = [5,3,6,2,4,null,7]
key = 3

    5
   / \
  3   6
 / \   \
2   4   7

Given key to delete is 3. So we find the node with value 3 and delete it.

One valid answer is [5,4,6,2,null,null,7], shown in the following BST.

    5
   / \
  4   6
 /     \
2       7

Another valid answer is [5,2,6,null,4,null,7].

    5
   / \
  2   6
   \   \
    4   7
```



在搜索二叉树中删除节点分两步：

1. 在搜索二叉树中查找目标节点：
   - 如果当前节点的值大于目标节点的值，则在左子树中继续查找。
   - 如果当前节点的值小于目标节点的值，则在右子树中继续查找。
   - 如果当前节点的值等于目标节点的值，则进行删除操作。
2. 删除节点：
   - 如果当前节点的左子节点为null，则直接用右子节点替代当前节点。
   - 如果当前节点的右子节点为null，则自己用做子节点替代当前节点。
   - **如果当前节点的左右子节点都不为null，则在右子树中找到最小的节点替代该节点，然后在右子树中删除最小节点。**

```java
public TreeNode deleteNode(TreeNode root, int key) {
    if (root == null) return null;
    if (root.val > key) root.left = deleteNode(root.left, key);
    else if (root.val < key) root.right = deleteNode(root.right, key);
    else {
        if (root.left == null) return root.right;
        else if (root.right == null) return root.left;

        TreeNode minNode = findMin(root.right);
        root.val = minNode.val;
        root.right = deleteNode(root.right, root.val);
    }
    return root;
}
public TreeNode findMin(TreeNode root) {
    while (root.left != null) root = root.left;
    return root;
}
```

