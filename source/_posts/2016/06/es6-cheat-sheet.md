---
title: ES6 CHEAT SHEET
date: 2016-06-24 23:56:33
tags: 
- es6
---

用一页展示所有ES6(es2015)新特性——持续更新中。

### let
``` javascript
{
  let a = 10;
  let a = 10; // 报错【重复声明报错】
}
a // ReferenceError: a is not defined【块级作用域】
```
### const
``` javascript
const PI = 3.1415;
PI = 3; 
PI // 3.1415 不报错，赋值会默默失败【常量】
const PI = 3.1; // TypeError: Identifier 'PI' has already been declared【重复声明报错】
```




## 参考文献
* [《ECMAScript 6 入门》](http://es6.ruanyifeng.com/)
* [《ECMAScript® 2015 Language Specification》](http://www.ecma-international.org/ecma-262/6.0/)
* [ECMAScript 6 compat table](http://kangax.github.io/compat-table/es6/)