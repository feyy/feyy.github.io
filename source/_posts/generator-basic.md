---
title: Generator Basics
date: 2016-01-15 00:24:53
tags:
- javascript
- ES6
- generator
---

首先来看一端js代码。

```javascript
                                        });
                                    });
                                });
                            });
                        });
                    });
                });
            });
        });
    });
});
```
是不是很眼熟呢？没错，不要怀疑，或多或少，或大或小的都存在这样的代码吧。  
异步编程是js的核心思想，ES6之前[Javascript异步编程的4种方法](http://www.ruanyifeng.com/blog/2012/12/asynchronous%EF%BC%BFjavascript.html)：

* 回调函数
* 事件监听
* 发布/订阅
* Promise对象

ES6的Generator将JavaScript异步编程带入了一个全新的阶段，ES7的Async函数更是提出了异步编程的终极解决方案。  
<!-- more -->
## 运行环境
node v0.11 可以使用 (node --harmony)

## 基本概念
### 函数定义
Generator函数是有两个特征：

* 一是，function命令与函数名之间有一个星号；
* 二是，函数体内部使用yield[/jiːld/]语句，定义不同的内部状态。

```javascript
function* helloWorldGenerator() {
    yield 'hello';
    yield 'world';
    return 'ending';
}
```
**ES6没有规定，function关键字与函数名之间的星号，写在哪个位置。推荐如上所示，紧跟在function关键字之后**
### 函数调用
Generator函数的调用方法与普通函数一样，也是在函数名后面加上一对圆括号。不同的是，调用Generator函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象，也就是一个[遍历器对象](http://es6.ruanyifeng.com/#docs/iterator)（Iterator Object）。

必须调用遍历器对象的next方法，使得指针移向下一个状态。每次调用next方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个yield语句（或return语句）为止。换言之，Generator函数是分段执行的，yield语句是暂停执行的标记，而next方法可以恢复执行。

```javascript
var hw = helloWorldGenerator();

hw.next()   // { value: 'hello', done: false }

hw.next()   // { value: 'world', done: false }

hw.next()   // { value: 'ending', done: true }

hw.next()   // { value: undefined, done: true }
```
## yield

* yield语句不能用在普通函数中，只能用在generator函数中，否则会报错；
* yield语句如果用在一个表达式之中，必须放在圆括号里面；  

```javascript
console.log('Hello' + yield); // SyntaxError
console.log('Hello' + yield 123); // SyntaxError

console.log('Hello' + (yield)); // OK
console.log('Hello' + (yield 123)); // OK
```
* yield语句用作函数参数或赋值表达式的右边，可以不加括号。

```javascript
foo(yield 'a', yield 'b'); // OK
let input = yield; // OK
```
* yield句本身没有返回值，或者说总是返回undefined。next方法可以带一个参数，该参数就会被当作上一个yield语句的返回值。

```javascript
function* foo(x) {
    var y = 2 * (yield (x + 1));
    var z = yield (y / 3);
    return (x + y + z);
}

var a = foo(5);
a.next() // Object{value:6, done:false}
a.next() // Object{value:NaN, done:false}
a.next() // Object{value:NaN, done:false}

var b = foo(5);
b.next() // { value:6, done:false }
b.next(12) // { value:8, done:false }
b.next(13) // { value:42, done:true }
```

## next
遍历器对象的next方法的运行逻辑如下。

（1）遇到yield语句，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值。

（2）下一次调用next方法时，再继续往下执行，直到遇到下一个yield语句。

（3）如果没有再遇到新的yield语句，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值，作为返回的对象的value属性值。

（4）如果该函数没有return语句，则返回的对象的value属性值为undefined。

**yield语句不能用在普通函数中，否则会报错**

## Generator.prototype
Generator函数返回一个Generator实例，可以通过这个实例调用原型链上的方法。

### Generator.prototype.throw()
* 可以在函数体外抛出错误，然后在Generator函数体内捕获。
* 如果Generator函数内部没有部署try...catch代码块，那么throw方法抛出的错误，将被外部try...catch代码块捕获。
* 如果Generator函数内部部署了try...catch代码块，那么遍历器的throw方法抛出的错误，不影响下一次遍历，否则遍历直接终止。
* Generator函数内抛出的错误，也可以被函数体外的catch捕获。
* 一旦Generator执行过程中抛出错误，就不会再执行下去了。如果此后还调用next方法，将返回一个value属性等于undefined、done属性等于true的对象，即JavaScript引擎认为这个Generator已经运行结束了。

### Generator.prototype.return()
* 可以返回给定的值，并且终结遍历Generator函数。
* 如果return方法调用时，不提供参数，则返回值的vaule属性为undefined。
* 如果Generator函数内部有try...finally代码块，那么return方法会推迟到finally代码块执行完再执行。

## 参考文献
* [Generator 函数 - ECMAScript 6入门](http://es6.ruanyifeng.com/#docs/generator)