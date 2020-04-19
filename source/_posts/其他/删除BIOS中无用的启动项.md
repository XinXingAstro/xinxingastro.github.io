---
title: 删除BIOS中无用的启动项
tags:
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-07-09 18:09:24
categories: 其他
---

在卸载完Ubuntu后，重启电脑发现Boot Menu中还有Ubuntu的启动项，这篇文章告诉你正确删除无用启动项的方法。

<!-- more -->

---

### 清除BIOS中无用的引导项

我们在装完Ubuntu系统后将其卸载，重启时发现BIOS的Boot Menu中还有Ubuntu系统的引导，Boot Menu中存在无用的启动项是因为电脑的EFI分区中存在该系统的引导文件，由于正常情况下EFI分区是不可见的，所以我们需要使用windows中的diskpart工具给EFI分区分配盘符，先让它在资源管理器中出现，然后我们再使用管理员权限进入EFI分区，删除分区中的多余文件，操作步骤如下：

1）右键开始菜单，运行，`diskpart`

2）`list disk`列出所有磁盘信息。

![bios01-1179206](bios01-1179206.png)

3）`select disk 0`要选中系统安装盘，我这里只有一个盘所以选0。

4）`list partition`列出磁盘中的分区信息。

![bios02](bios02.png)

5）其中类型为系统的分区就是EFI分区一般大小再一二百MB，`select partition 2`选中EFI分区。

6）`assign letter=p`给EFI分区分配盘符，这样就可以在资源管理器中看到EFI分区了，但是这时候我们是无法进入该分区的，因为权限不够，我们需要使用管理员权限进入该分区。

7）可以用管理员权限随便打开一个软件，比如winrar，7z。然后通过该软件就可以进入该分区了。

![bios03](bios03.png)

这是我用管理员权限进入EFI分区后7z，里面的内容，在EFI文件夹中会有一个ubuntu文件夹，或者grub文件夹，我们将其右键删除即可。

8）然后我们回到命令行输入`remove letter=p`删除EFI分区的盘符。

重启时进入Boot Menu，发现之前装过的Ubuntu系统引导就没有了。

>参考：[彻底删除Ubuntu EFI分区及启动项](https://blog.csdn.net/mtllyb/article/details/78635757)

