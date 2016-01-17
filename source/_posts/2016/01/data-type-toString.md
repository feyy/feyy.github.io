---
title: javascript中的数据类型判断
date: 2016-01-17 02:22:16
tags:
- javascript
---
靠，周末比平时还累，好不容易补完了昨天的一篇...看来今天又要水一篇了😭。
## typeof
| 类型                                             | 结构        |
|--------------------------------------------------|-------------|
| Undefined                                        | undefined   |
| Null                                             | object (😣) |
| 布尔值                                           | boolean     |
| 数值                                             | number      |
| 字符串                                           | string      |
| Symbol (ECMAScript 6 新增)                       | symbol      |
| 函数对象 (implements [[Call]] in ECMA-262 terms) | function    |
| 任何其他对象                                     | object      |

**能认识的就这么多了，君看着办吧**
## instanceof
注意了这货不能跨环境，如多个frame或window。

```javascript
object instanceof constructor
```

**我们玩个真心话大冒险吧，你随便问，不过我只能回答“是”或者“否”，你开始吧**
## toString()
猪脚上传...🐷...长相很一般吗...
<!-- more -->
`toString`方法的主要用途是返回对象的字符串形式。
但使用call方法，可以在任意值上调用Object.prototype.toString方法，从而帮助我们判断这个值的类型。

* 数值：返回[object Number]。
* 字符串：返回[object String]。
* 布尔值：返回[object Boolean]。
* undefined：返回[object Undefined]。
* null：返回[object Null]。
* 数组：返回[object Array]。
* arguments对象：返回[object Arguments]。
* 函数：返回[object Function]。
* Error对象：返回[object Error]。
* Date对象：返回[object Date]。
* RegExp对象：返回[object RegExp]。
* 其他对象：返回[object " + 构造函数的名称 + "]。

```javascript
Object.prototype.toString.call(2)           // "[object Number]"
Object.prototype.toString.call('')          // "[object String]"
Object.prototype.toString.call(true)        // "[object Boolean]"
Object.prototype.toString.call(undefined)   // "[object Undefined]"
Object.prototype.toString.call(null)        // "[object Null]"
Object.prototype.toString.call(Math)        // "[object Math]"
Object.prototype.toString.call({})          // "[object Object]"
Object.prototype.toString.call([])          // "[object Array]"
```




