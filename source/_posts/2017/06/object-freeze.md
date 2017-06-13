---
title: object冻结
date: 2017-06-12 23:56:33
tags:
- javascript
- object
---

javascript如何实现object冻结，保证一个不可变的object。

### const 常量

const声明的常量的值不能通过重新赋值来改变，并且不能重新声明[声明时必须初始化]。针对的是这个变量本身，而不是变量内部的值，可直接理解为针对变量名。

``` javascript
const OBJ = {test: 123};

// 重复声明常量会报错
var OBJ;
let OBJ;
const OBJ = {test: 123}

// 给常量赋值会报错
OBJ = 'test';

// 对里面的值的操作确没有限制
OBJ.test = 1234;
Object.getOwnPropertyDescriptor(OBJ, 'test'); // {value: 1234, writable: true, enumerable: true, configurable: true}
```

### extensible 扩展性 [Object.isExtensible, Object.preventExtensions]

如果一个对象可以添加新的属性，则这个对象是可扩展的。

``` javascript
const OBJ = {test: 123};

// 监测对象是否可扩展
Object.isExtensible(OBJ); // true

OBJ.test1 = 123; //{test: 123, test1: 123}

// 使对象不可扩展
Object.preventExtensions(OBJ);

OBJ.test2 = 123; //{test: 123, test1: 123}

Object.isExtensible(OBJ); // false
```
* 不可扩展的对象的属性通常仍然可以被删除。
* 尝试给一个不可扩展对象添加新属性的操作将会失败，不过可能是静默失败，也可能会抛出 TypeError 异常（严格模式下）。
* Object.preventExtensions 只能阻止一个对象不能再添加新的自身属性，仍然可以为该对象的原型添加属性。

### seal 密封性 [Object.isSealed, Object.seal]

密封对象是指那些不能添加新的属性，不能删除已有属性，以及不能修改已有属性的可枚举性（enumerable）、可配置性（configurable）[属性是否可删除以及属性描述特性是否可修改]、可写性（writable），但可能可以修改已有属性的值的对象。

```javascript
const OBJ = {test:123};

// 使对象不可扩展
Object.preventExtensions(OBJ);

Object.isSealed(OBJ); // false

Object.defineProperty(OBJ, "test", {configurable : false});

Object.isSealed(OBJ); // true, 也可以直接使用Object.seal(OBJ);

OBJ.test = 1234; // {test: 1234}

```

* 被密封的对象，就是在不可扩展基础上讲属性描述符configurable设置为false;
* 同时，被密封的对象，仍然有机会改变属性的值。只不过对于此对象本身而言，不可以再扩展新的属性，不可以更改已有属性的配置信息。

### freeze 冻结 [Object.isFrozen, Object.freeze]

冻结（frozen）是指它不可扩展，所有属性都是不可配置的（non-configurable），且所有数据属性（data properties, 另一种是访问器属性）都是不可写的（non-writable）。

```javascript
const OBJ = {test:123};

Object.freeze(OBJ);

Object.getOwnPropertyDescriptor(OBJ, 'test'); // {value: 123, writable: false, enumerable: true, configurable: false}

// 对冻结对象的任何操作都会失败
OBJ.name = "Eros"; // 改写属性值，非严格模式下静默失败;
OBJ.age = 18; // 扩展属性值，非严格模式下静默失败;
Object.defineProperty(OBJ,"test",{value: "Eros"}); // 使用defineProperty会直接报错
```

* 这种层面的冻结，只是浅冻结。如果对象里面还嵌套有对象，那么这个内部对象丝毫不受影响。

### 递归冻结

```javascript
Object.prototype.deepFreeze = Object.prototype.deepFreeze || function (o){
    var prop, propKey;
    Object.freeze(o); // 首先冻结第一层对象
    for (propKey in o){
        prop = o[propKey];
        if(!o.hasOwnProperty(propKey) || !(typeof prop === "object") || Object.isFrozen(prop)){
            continue;
        }
        deepFreeze(prop); // 递归
    }
}
```


## 参考文献
* [《JS冻结对象的《人间词话》 完美实现究竟有几层境界？》](http://www.jianshu.com/p/23c003b044a5)