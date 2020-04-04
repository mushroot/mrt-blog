---
title: 在VUEJS中使用require引入动态图片
date: 2020-03-20 15:05:03
tags: vue
---
在 VUEJS 中使用require引入包含变量的图片时，一般会出现如下警告：
> Critical dependency: require function is used in a way in which dependencies cannot be statically extracted

这是因为 vue 在编译的时候并没有数据传入，会因找不到上下文环境而查找失败。

**此时可以通过 require.context() 函数来创建自己的 context。**

可以给这个函数传入三个参数：
- 一个要搜索的目录
- 一个标记表示是否还搜索其子目录
- 以及一个匹配文件的正则表达式。

webpack 会在构建中解析代码中的 require.context() 。

语法如下：

```
require.context(directory, useSubdirectories = false, regExp = /^\.\//);
```

示例：
```
require.context('./test', false, /\.test\.js$/);
// （创建出）一个 context，其中文件来自 test 目录，request 以 `.test.js` 结尾。

require.context('../', true, /\.stories\.js$/);
// （创建出）一个 context，其中所有文件都来自父文件夹及其所有子级文件夹，request 以 `.stories.js` 结尾。
```

[详细参见文档](https://webpack.docschina.org/guides/dependency-management/#%E5%B8%A6%E8%A1%A8%E8%BE%BE%E5%BC%8F%E7%9A%84-require-%E8%AF%AD%E5%8F%A5)
