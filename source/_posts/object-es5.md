---
title: Object in ES5
date: 2016-01-16 03:21:21
tags:
- object
- es5
---
## Object.create(proto[, descriptors])

```javascript
// 指定原型创建空对象
Object.create(proto)
// 指定原型和属性秒速来创建一个对象
Object.create(proto, descriptors)

// example
var obj = Object.create({x: 1}, {
    z: {
        value: 3,
        writable: true,
        enumerable: true,
        configurable:true
    }
});
```

### 属性描述

```javascript
{ 
    value: 3,               // 属性值
    writable: true,         // 属性是否可写, 默认为false
    enumerable: true,       // 属性是否可枚举, 默认为false [当使用for/in语句时，该property是否会被枚举]
    configurable: true      // 属性是否可配置, 默认为false [该property的属性是否可以修改(如由不可写改为可写)，property是否可以删除]
}
```

**使用 `obj.a = 123;` 创建的属性writable, enumerable, configurable属性均为true**

```javascript
// 拥有getter和setter的属性是否可读写取决于是否有get和set函数，因此没有value和writable属性
{
    get: function(){...},
    set: function(){...},
    enumerable: true,
    configurable: true
}
// example
var o ={
  $n : 5,
  get next(){return this.$n++;},
  set next(n) {this.$n = n;}
};
```
<!-- more -->
## Object.defineProperties(obj, descriptors)
创建或配置对象的多个属性

```javascript
@param {object} obj 要在其上创建或者配置属性的对象
@param {object} descriptors 将属性名映射到属性描述符的对象
@return {object} obj 返回传入的obj对象
Object.defineProperties(obj,{
    a:{value:"a",writable:false,enumerable:true,configurable:true},
    b:{value:"b",writable:false,enumerable:true,configurable:true}
})
```

## Object.defineProperty(obj, name, desc)
创建或配置对象的一个属性
```javascript
@param {object} obj 要在其上创建或者配置属性的对象
@param {string} name 将属性名映射到属性描述符的对象
@param {object} desc 一个属性描述符对象，描述要创建的新属性或对现有属性的修改
@return {object} obj 返回传入的obj对象
Object.defineProperty(obj, "c", { 
    value: "c",
    writable:false, 
    enumerable:false, 
    configurable:true
})
```
## Object.getOwnPropertyDescriptor(obj, name)
查询一个属性的描述对象。  
返回对象指定属性的一个属性描述符对象，如果不存在指定属性则返回undefined.
## Object.getPrototypeOf(obj)
返回一个对象的原型
## obj1.isPrototypeOf(obj2)
判断当前对象是否为另一个对象的原型
## Object.getOwnPropertyNames(obj)
返回非继承属性的名字。包括那些不可枚举的属性。
## Object.keys(obj)
返回对象的可枚举属性名.

```javascript
Object.keys({x: 1, y:2})
// ['x', 'y']
```
**getOwnPropertyNames与keys的区别在于属性是否可枚举**

```javascript
var a = Object.create({x:1,y:2}, {z:{value:3}});
Object.keys(a);
// []
Object.getOwnpropertyNames(a);
// ['z']
```
## object.propertyIsEnumerable(propname)
检测某个属性是否可枚举，即在for/in 中 循环可见
## Object.preventExtensions(obj)
禁止在一个对象上添加新的属性。不可扩展
## Object.isExtensible(obj)
判断某个对象上是否可以添加新属性  
返回: 能添加为true|不能为false  
**所有的对象在创建的时候都是可扩展的，直到他们被传入 Object.preventExtensions(o) Object.seal(o) 或 Object.freeze(o);**
## Object.seal(obj)
封闭对象。阻止添加或删除对象的属性
## Object.isSealed(obj)
判断一个对象是否是封闭的。  
如果不可以向一个对象添加新的（非继承）属性，并且现有的（非继承）属性不可删除，则是封闭的。
## Object.freeze(obj)
将一个对象设为不可改变,不会影响继承属性
## Object.isFrozen(obj)
判断对象是否不可改变, 如果o已冻结并不改变则为true;否则为false;

**限制程度：preventExtensions < seal < freeze**
## 参考文档
* [javascript中Object使用详解](http://www.jb51.net/article/60358.htm)
* [Object对象](http://javascript.ruanyifeng.com/stdlib/object.html)
