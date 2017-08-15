# 知识点

* [null](#1.1 无中生有)
* [_proto_与prototype](#21-proto-与-prototype)

---

## 1 归纳总结

#### 1.1 无中生有

![](/assets/part1-object-bord001.png)

> JS最初什么都没有。但是没有也是一个对象。没有的对象Null

![](/assets/part1-object-bord01.png)

&gt;

---

## 2 概念释义

#### 2.1 \__proto\__ 与 prototype

======================================================================================================

**参考1：**来源知乎 作者：doris  [链接](https://www.zhihu.com/question/34183746/answer/58068402)

关系图：

![](/assets/part1-object-bord-03.png)

**明确基础点**

1.在JS里，万物皆对象。方法（Function）是对象，方法的原型\(Function.prototype\)是对象。因此，它们都会具有对象共有的特点。  
即：对象具有属性\_\_proto\_\_，可称为隐式原型，一个对象的隐式原型指向构造该对象的构造函数的原型，这也保证了实例能够访问在构造函数原型中定义的属性和方法。

```js
function Study() {
  basic: '持续不断学习'
}
var study1 = new Study()
study1.__proto__ === Study.prototype // => true
```

2.方法\(Function\)  
方法\(或者被称为函数\)这个特殊的对象，除了和其他对象一样有上述\_proto\_属性之外，还有自己特有的属性——原型属性（prototype），这个属性是一个指针，指向一个对象，这个对象的用途就是包含所有实例共享的属性和方法（我们把这个对象叫做原型对象）。原型对象也有一个属性，叫做constructor，这个属性包含了一个指针，指回原构造函数。

```js
function Study() {
  basic: '持续不断学习'
}
var study1 = new Study()
study1.prototype // => undefined
Study.prototype.constructor === Study // => true

// 函数
function fn() {}
fn.prototype // => Object{} 包含[constructor],[__proto__]

// 函数表达式
const fn1 = function() {}
fn1.prototype // => Object{} 包含[constructor],[__proto__]
```

**解释上述图：**

1.构造函数Foo\(\)  
构造函数的原型属性Foo.prototype指向了原型对象，在原型对象里有共有的方法，所有构造函数声明的实例（这里是f1，f2）都可以共享这个方法。

2.原型对象Foo.prototype  
Foo.prototype保存着实例共享的方法，有一个指针constructor指回构造函数。

3.实例  
f1和f2是Foo这个对象的两个实例，这两个对象也有属性\_\_proto\_\_，指向构造函数的原型对象，这样子就可以像上面1所说的访问原型对象的所有方法啦。

另外：  
构造函数Foo\(\)除了是方法，也是对象啊，它也有\_\_proto\_\_属性，指向谁呢？  
指向它的构造函数的原型对象呗。函数的构造函数不就是Function嘛，因此这里的\_\_proto\_\_指向了Function.prototype。  
其实除了Foo\(\)，Function\(\), Object\(\)也是一样的道理。

原型对象也是对象啊，它的\_\_proto\_\_属性，又指向谁呢？

同理，指向它的构造函数的原型对象呗。这里是Object.prototype.

最后，Object.prototype的\_\_proto\_\_属性指向null。

```js
function Study() {
   basic: 'like it'
}
Study.__proto__ === Function.prototype // => true 指向Function.prototype
Study.prototype.__proto__ === Object.prototype // => true 原型对象的__proto__指向Object.prototype
Object.prototype.__proto__ === null // => true 对象原型的__proto__指向null

Object.__proto__ // => function() {} 控制台返回
Object.__proto__ === function() {} // => false
Object instanceof Function // => true Object是对象的构造函数，本身是一个函数
Object.prototype === Object.__proto__.__proto__ // => true Object构造函数，Object.__proto__是一个原型对象，
// 原型对象的__proto__，指向Object.prototype。Object.prototype.__proto__指向null
```

===============================================================================================

> prototype 是函数才有的属性，准确的说是构造函数才有的属性。\([普通函数和函数表达式都可以看成是构造函数](/docs/part1/object/basicfn-vs-construtor.md)\)
>
> \_\__proto_\_\_是所有JavaScript对象（包括函数）都有的属性。
>
> prototype 是规范中定义的属性，JS运行时环境必须实现。
>
> \_\__proto_\_\_不是一个标准属性，只有chrome的v8等部分引擎才支持。前者表示，在用某个构造函数实例化一个对象时，这个被实例化出来的对象的原型是谁。后者表示某个对象的原型。

[BackTop](#知识点)

---

## 参考文档

* [JavaScript 世界万物诞生记](https://zhuanlan.zhihu.com/p/22989691)    



