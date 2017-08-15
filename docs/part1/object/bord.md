# 知识点

* [null](#1.1 无中生有)
* [_proto_与prototype](#21-proto-与-prototype)

---

## 1 归纳总结

#### 1.1 无中生有

![](/assets/part1-object-bord001.png)

> JS最初什么都没有。但是没有也是一个对象。没有的对象Null

![](/assets/part1-object-bord01.png)

>

---

## 2 概念释义

#### 2.1 \__proto\__ 与 prototype

**参考1：**来源知乎。[链接](https://www.zhihu.com/question/34183746/answer/58068402)

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
study1.__proto__ == Study.prototype // => true
```

2.方法\(Function\)  
方法这个特殊的对象，除了和其他对象一样有上述\_proto\_属性之外，还有自己特有的属性——原型属性（prototype），这个属性是一个指针，指向一个对象，这个对象的用途就是包含所有实例共享的属性和方法（我们把这个对象叫做原型对象）。原型对象也有一个属性，叫做constructor，这个属性包含了一个指针，指回原构造函数。

  


  


作者：doris

  


链接：https://www.zhihu.com/question/34183746/answer/58155878

  


来源：知乎

  


著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

> prototype 是函数才有的属性，准确的说是构造函数才有的属性。
>
> \_\__proto\_\__是所有JavaScript对象（包括函数）都有的属性。
>
> prototype 是规范中定义的属性，JS运行时环境必须实现。
>
> \_\__proto\_\__不是一个标准属性，只有chrome的v8等部分引擎才支持。前者表示，在用某个构造函数实例化一个对象时，这个被实例化出来的对象的原型是谁。后者表示某个对象的原型。

[BackTop](#知识点)

---

## 参考文档

* [JavaScript 世界万物诞生记](https://zhuanlan.zhihu.com/p/22989691)    



