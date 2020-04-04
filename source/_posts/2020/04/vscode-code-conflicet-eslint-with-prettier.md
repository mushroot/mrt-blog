---
title: VScode Eslint 和 prettiner 代码风格冲突
date: 2020-04-04 14:50:26
tags: vscode
---

> 最近更新项目之后执行 npm run lint 会出现如下错误，那是因为新版的 prettier 和 eslint 的默认风格不一致，
> 我们需要手动指定相对应的风格。
```
20 error code ELIFECYCLE
21 error errno 1
22 error PinPaiChuan@0.0.1 lint: `eslint .`
22 error Exit status 1
23 error Failed at the PinPaiChuan@0.0.1 lint script.
23 error This is probably not a problem with npm. There is likely additional logging output above.
24 verbose exit [ 1, true ]
```

# 修改 .eslintrc.js
在项目根目录下找到或者新建 .eslintrc.js 文件，并在 rule 中指定使用单引号。
```
rules: {
    quotes: ['error', 'single'],
  }
```

# 修改 .prettierrc
在项目根目录找到或者新建文件 .prettierrc, 指定和 eslint 一样的风格。

```
{
  "eslintIntegration": true, // 开启 eslint 支持
  "singleQuote": true, // 使用单引号
  "semi": true // 末尾使用分号结尾
}
```

> 上面两种方法同样可以去修改 vscode 的 setting.json 实现。但是一般建议直接在项目中指定。