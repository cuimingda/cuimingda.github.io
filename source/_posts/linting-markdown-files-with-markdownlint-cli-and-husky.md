---
title: 基于markdownlint和husky校验markdown文件
date: 2022-05-23 21:07:54
tags:
---

基于Hexo来写博客，本质上就是写大量的markdown文档，那是不是有类似eslint一样的工具，也能对markdown文件进行校验呢，答案是确定的，工具的名称就是``markdownlint``。

首先安装``markdownlint``的命令行工具：

```shell
brew install markdownlint-cli
```

然后安装``husky``：

```shell
npm install husky --save-dev
```

初始化

```shell
npm set-script prepare "husky install"
npm run prepare
```

增加一个钩子

```shell
npx husky add .husky/pre-commit "markdownlint source"
git add .husky/pre-commit
```

这时候，只要我们再次提交commit，就会检查source目录中的所有``markdown``文件是不是符合要求，如果不符合要求，其实还可以用快速修理

```shell
markdownlint -f source
```

具体有哪些规则，可以直接到markdownlint的页面查询：
<https://github.com/DavidAnson/markdownlint/blob/HEAD/doc/Rules.md>

#### References

* <https://github.com/DavidAnson/markdownlint>
* <https://github.com/typicode/husky>
