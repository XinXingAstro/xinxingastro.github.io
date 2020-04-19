---
title: 为什么重写equals方法就必须重写hashCode方法
tags:
  - null
  - null
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2017-08-14 16:26:25
categories: Java
---

SUN的官方文档中规定，如果重定义equals方法，就必须重定义hashCode方法,以便用户可以将对象插入到散列(哈希)表中。

<!-- more -->

---

在定义集合时往往会自定义覆盖hashCode和equals方法，是因为需要遵守equals的特性规则：

> equals与hashCode的定义必须一致，两个对象equals为true,就必须有相同的hashCode。反之则不成立。

例如：如果定义的equals比较的是雇员ID，那么hashCode就需要散列ID，而不是雇员的姓名或住址，用不到哈希表可以不复写，除非你确认类的对象不会放入 HashSet,Hashtable,HashMap.等哈希表集合中。 即使暂时用不到哈希表，为其提供一个与equals一致的hashCode方法也没什么坏处，万一以后用到了呢？ 所以，在定义元素类时，为了避免不必要的麻烦，复写equals方法时通常有必要复写hashCode方法，以保证判定结果的一致性。