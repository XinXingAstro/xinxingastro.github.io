---
title: vim使用技巧
tags:
  - vim
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2017-06-05 23:04:24
categories: 其他
---

vim常用技巧总结。

<!-- more -->

---

#### 批量替换

```
:s/XXX/YYY/g
其中XXX是需要替换的字符串,YYY是替换后的字符串
以上这句只对当前行进行替换,如果需要进行全局替换,则要:
%s/XXX/YYY/g

如果需要对指定部分进行替换,可以用V进入visual模式,再进行
:s/XXX/YYY/g
或者可以指定行数对指定范围进行替换:
:100, 102s/XXX/YYY/g
```

#### Visual模式

```
visual模式就是选择模式
v:进入单个字符选择模式
V:进入行选择模式
```

