---
title: Java中获取随机数的方法
tags:
  - java
comments: true
mathjax: true
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-04-28 17:09:12
categories: Java
---

最常用的有两种方法获取随机数。

<!-- more -->

## 使用Math.random()方法

`Math.random()`函数获取一个在`[0.0, 1.0)`之间的double类型的随机数。

例：获取在`[0.0, 10.0)`之间int类型的随机数：

```java
double r = Math.random();
int ans = (int)(r*10);
```

---

## 使用Random类获取随机数

Random类中有很多获得随机数的方法，如：

- `int nextInt()`方法返回在int类型的$[-2^{31}\sim2^{31}-1]$的随机数。
- `int nextInt(int bound)`方法返回int类型的[0, bound)之间的随机数。

例：获取在`[0.0, 10.0)`之间int类型的随机数：

```java
Random r = new Random();
int ans = r.nextInt(10);
```

---

## 使用UUID类获取UUID

```java
UUID uuid = UUID.randomUUID();
```

UUID得到的是一个随机字符串。



>