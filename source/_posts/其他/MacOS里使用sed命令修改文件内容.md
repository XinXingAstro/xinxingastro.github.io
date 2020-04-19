---
title: MacOS里使用sed命令修改文件内容
tags:
  - MacOS
  - sed
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2017-06-04 21:59:37
categories: 其他
---

Mac上sed命令使用的一个小例子。

<!-- more -->

---

Mac上-i命令和Linux上有一点不同：

>-i <u>extension</u>
>
>Edit files in-place, saving backups with the specified extension.  If a zero-length extension  is given, no backup will be saved.  It is not recommended to give a zero-length extension when  in-place editing files, as you risk corruption or partial content in situations where disk space is exhausted, etc.

Mac上-i后面需要加一个字符串，表示备份文件的扩展名，如果添加一个空串，就不会有备份文件。

一个小例子：

```shell
#!/bin/bash
cd ~/myProjects/Blog
hexo new $1
if  [ "$2" ] ;then 
    sed -i "" "s/categories:/categories: $2/g" source/_posts/$1.md
    mv source/_posts/$1.md source/_posts/$2/
    open source/_posts/$2/$1.md
else
    open source/_posts/$1.md
fi
```

用hexo创建一个博客文件，如果命令后面跟了分类信息，则将分类信息补充到categories: 后面。

注意：如果要在字符串中添加传入参数，则必须使用双引号，使用单引号无法引用传入参数。如上面如果将`"s/categories:/categories: $2/g"` 换成`'s/categories:/categories: $2/g'`，在文件中只会写入`categories: $2`。