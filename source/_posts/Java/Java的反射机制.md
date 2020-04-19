---
title: Java的反射机制
tags:
  - null
  - null
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-07-21 10:59:05
categories: Java
---

Java反射机制的知识点总结。

<!-- more -->

---

### 反射的概念

在Java运行时环境中，对于任意一个类我们可以利用反射机制知道这个类有哪些属性和方法，对于任意一个对象我们可以利用反射调用它的任意一个方法，这种动态获取类的信息以及动态调用对象的方法的功能来自于Java的反射(Reflection)机制。

Java反射机制主要提供了一下功能：

- 在运行时判断任意一个对象所属的类。
- 在运行时构造任意一个类的对象。
- 在运行时判断任意一个类所具有的成员变量和方法。
- 在运行时调用任意一个对象的方法。

> 什么是动态语言？
>
> 动态语言：就是程序运行时，允许改变程序结构或变量类型的语言。
>
> 最常见的动态语言有：JavaScript、Python、Ruby等。

虽然Java不是动态语言，但是Java的反射机制，让Java也具有了一定的动态性。

### Class类

在Java程序运行期间，Java运行时系统始终为所有对象维护一个叫作**运行时**的类型标识，这个信息跟踪着每个对象所属的类，JVM利用运行时里的类型信息选择相应的方法执行。我们可以通过专门的Java类访问这些运行时信息，保存这些信息的类就是**Class**类。**java.lang.Class**是一个特殊的类，它用于封装被装入到JVM中的类或接口的信息。

Class类没有构造方法，我们不能显示的构造一个Class对象，Class对象是在一个类或接口被加载时，JVM通过类加载器中的defineClass方法自动构造的，我们可以通过这个Class对象访问这个类的信息。JVM通过这个Class对象来构造对应类的普通对象。

#### 获取Class对象

##### 通过Object类的getClass()方法获取Class对象

```java
Date date1 = new Date();
Date date2 = new Date();
Class c1 = date1.getClass();
Class c2 = date2.getClass();
System.out.println(c1.getName()); // java.util.Date
System.out.println(c1 == c2); // true
```

当一个对象被其父类的引用或其实现的接口的应用指向时，getClass()方法返回的是对象实际类型的Class对象。

```java
List list = new ArrayList();
System.out.println(list.getClass().getName()); // java.util.ArrayList
```

有时候可以用这个方法了解一个对象的实际类型，例如：

```java
Set set = new HashSet();
Iterator it = set.iterator();
System.out.println(it.getClass().getName()); // java.util.HashMap$KeyIterator
```

从代码可以看出，HashSet的iterator方法返回的是实现了Iterator接口的HashMap内部类（KeyIterator）对象。 因为抽象类和接口不可能实例化对象，因此不能通过Object的getClass方法获得与抽象类和接口关联的Class对象。

##### 使用.class方法获取Class对象

```java
Class clazz = String.class;
System.out.println(class.getName()); // java.lang.String
```

这个方法可以直接获得与指定类关联的Class对象，而并不需要有该类的对象存在。

##### 使用Class.forName(String className)方法获取Class对象

该方法可以根据字符串参数所指定的类名获取与该类关联的Class对象。如果该类还没有被装入，该方法会将该类装入JVM。当该方法无法获取需要装入的类时（例如，在当前类路径中不存在这个类），会抛出ClassNotFoundException异常。

注意：className需要是类的全限定名。

```java
Class clazz = Class.forName("com.xinxing.Test");
```

上面语句执行后，JVM首先会将Test类装入，然后返回对应的Class对象。该方法通常用于在程序运行时根据类名动态的载入类，并获取对应的Class对象。