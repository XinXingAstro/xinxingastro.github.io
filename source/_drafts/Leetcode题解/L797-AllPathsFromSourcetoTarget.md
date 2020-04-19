---
title: Leetcode797. All Paths From Source to Target
tags:
  - 图
  - DFS
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-20 16:00:43
categories: Leetcode题解
---

给出一个有环图，找出所有从第0个节点到第N-1个节点的路径。

<!-- more -->

---

### 797. All Paths From Source to Target

Given a directed, acyclic graph of `N` nodes.  Find all possible paths from node `0` to node `N-1`, and return them in any order.

The graph is given as follows:  the nodes are 0, 1, ..., graph.length - 1.  graph[i] is a list of all nodes j for which the edge (i, j) exists.

```
Example:
Input: [[1,2], [3], [3], []] 
Output: [[0,1,3],[0,2,3]] 
Explanation: The graph looks like this:
0--->1
|    |
v    v
2--->3
There are two paths: 0 -> 1 -> 3 and 0 -> 2 -> 3.
```

**Note:**

- The number of nodes in the graph will be in the range `[2, 15]`.
- You can print different paths in any order, but you should keep the order of nodes inside one path.

---

#### 算法：深度优先搜索

使用图的优先搜索算法搜索从第0个节点开始的每条路径，如果有一个路径可以到达第N-1个节点，则将这条路径放入结果集。

```java
List<List<Integer>> res = new ArrayList<>();
public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
    if (graph == null || graph.length == 0)
        return res;
    List<Integer> path = new ArrayList<>();
    path.add(0);
    DFS(graph, 0, path);
    return res;
}
public void DFS(int[][] graph, int node, List<Integer> path) {
    if (node == graph.length - 1) {
        res.add(new ArrayList<Integer>(path));
        return;
    }

    for (int nextNode : graph[node]) {
        path.add(nextNode);
        DFS(graph, nextNode, path);
        path.remove(path.size() - 1);
    }
}
```

