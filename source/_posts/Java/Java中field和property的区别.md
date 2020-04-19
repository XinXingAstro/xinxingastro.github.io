---
title: Java中field和property的区别
tags:
  - null
  - null
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-07-21 22:46:47
categories: Java
---

field和property中文的翻译差不多，实际上是两个不同的概念。

<!-- more -->

---

> field: A data member of a class. Unless specified otherwise, a field is not static.
>
> property: Characteristics of an object that users can set, such as the color of a window.
>
> 摘自：[Glossary of Terms](https://docs.oracle.com/javase/tutorial/information/glossary.html#P)

我的理解是，field指那些在类的内部，不能被外界看到的私有成员变量。property指的是在对象内部可以被用户设置或者读取的属性，比如那些有getter/setter方法的属性。

![fieldvsproperty-2232158](fieldvsproperty.png)

如上图所示我们新建一个Person类，在没有设置getter/setter方法时，name和age都是field，然后我给name添加getter/setter方法后，getName(), setName()和name就变成了一个property。