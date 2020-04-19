# 创建分类页面
新建一个页面，命名为 categories 命令如下：
```shell
 hexo new page "categories"
```
编辑/source/categories/index.md，将页面的类型设置为 categories ，主题将自动为这个页面显示所有分类。
```
---
 title: 分类
 date: 2014-12-22 12:39:04
 type: "categories"
 ---
```
注意：如果有启用多说 或者 Disqus 评论，默认页面也会带有评论。需要关闭的话，请添加字段 comments 并将值设置为 false，如：
```
---
 title: 分类
 date: 2014-12-22 12:39:04
 type: "categories"
 comments: false
 ---
```
在`theme/ _config.yml` 的 menu 中的添加:
```yaml
 menu:
   categories: /categories/ || fa fa-th
```

`||`之后的是分类页面的图标，图标名称来自于[Font Awesome](http://fontawesome.io/icons/)。若没有配置图标，默认会使用问号图标。

在`hexo>theme>next>languages>zh-CN.yml`的menu下添加字符映射：

```yml
menu:
  categories: 分类
```

