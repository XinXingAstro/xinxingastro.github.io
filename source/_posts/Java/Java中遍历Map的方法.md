---
title: Java中遍历Map的方法
tags:
  - map
  - java
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-05-03 16:37:53
categories: Java
---

如HashMap, TreeMap, ConcurrentHashMap等。

<!-- more -->

---

### 方法1: 使用Iterator遍历

```java
Map<Integer, Integer> map = new HashMap<>();
Iterator<Map.Entry<Integer, Integer>> entries = map.entrySet().iterator();
while (entries.hasNext()) {
    Map.Entry<Integer, Integer> entry = entries.next();
    System.out.println("Key = " + entry.getKey() + ", Value = " + entry.getValue());
}
```

使用Iterator迭代map的一个优势是：在迭代过程中可以使用iterator.remover()正常删除map中的entry, 如果在for-each循环中删除entry，系统会报`unpredictable resultes`异常。

### 方法2: for-each遍历map.entrySet()

```java
Map<Integer, Integer> map = new HashMap<>();
for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
    System.out.println("Key = " + entry.getKey() + ", Value = " + entry.getValue());
}
```

### 方法3: for-each遍历map.keySet(), map.values

```java
Map<Integer, Integer> map = new HashMap<>();

for (Integer key : map.keySet()) {
    System.out.println("Key = " + key);
    Integer value = map.get(key); //效率低
    System.out.println("Value = " + value);
}

for (Integer value : map.values()) {
    System.out.println("Value = " + value);
}
```