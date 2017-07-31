## 7 私有方法 es6 class不提供私有方法，只能模拟实现

```js
// es6不提供私有方法，必须通过变通方法实现
        // 实现方式1
        class Dog {
            // 公有方法
            sayName(name) {
                this_bar(name)
            }
            // 私有方法
            _bar(baz) {
                return this.snaf = baz
            }
        }
        // 实现方式2
        class Car {
            // 公有方法
            showPrice(length) {
                carLength.call(this, length)
            }
        }
        // 私有方法2
        function carLength(length) {
            return this.snaf = length
        }

        // 实现方法3
        const cbar = Symbol('bar')
        const csnaf = Symbol('snaf')

        export default class myClass {
            // 公有方法
            foo(baz) {
                this[cbar](baz)
            }
            // 私有方法
            [cbar](baz) {
                return this[csnaf] = baz
            }
        }
```

> ES5 私有方法：在构造函数中通过var let const 定义的属性-- 私有方法本身是可以访问类内部的所有属性的，即私有属性和公有属性。但是私有方法是不可以在类的外部被调用
>
> ES5 公有方法：在构造函数的prototype定义的方法，可以被外部调用，但是不能访问私有属性和方法
>
> ES5 特权方法：可以访问私有属性和私有方法的公有方法

私有方法：

> 在某些情景下，你可能希望限制某些属性和方法的暴露程度，使他们不能通过对象实例本身被访问、修改或调用。许多传统语言可以将属性和方法定义为公有、私有或者受保护的。私有变量或方法在类定义之外不能被进行读写；受保护的变量不能被直接访问；但可以通过一个包装方法对其读写。在[js](http://lib.csdn.net/base/javascript)中并没有具体的语法来定义私有或受保护的变量和方法，不过我们可以对声明“类”的方法做一些改变，从而限制对属性和变量的访问。

特权方法：

> 在构造函数中通过var let const定义的变量其作用域局限于该构造函数内——在prototype上定义的方法无法访问这个变量，因为这些方法有其自己的作用域。想要通过公有的方法来访问私有的变量，需要创建一个包含两个作用域的新作用域。为此，我们可以创建一个自我执行的函数，称为闭包。代码如下。

## 8 私有属性 es6通过\#表示私有属性

---

## 9 类方法中this的指向

---

## 10 name属性返回class关键字后面的类名

---

## 11 Class的取整函数\(getter\)和存值函数\(setter\)

```js
// 类中通过 get set 设置取值函数和存值函数
        class Cat {
            get prop() {
                console.log( 'getter-取值')
            }
            set prop(value) {
                console.log( 'setter:'+ value )
            }
        }
        const myCat = new Cat()
        myCat.prop = 123
        myCat.prop
```

## get set实际设置在 属性的描述对象上面，和es5相似

---

## 12 Class的Generator方法\(某方法前加 \*表示该方法为Generator函数\)

---

## 13 Class的静态方法\(构造函数的方法\)

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上`static`关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。（**构造函数自有的方法，例如A.sayName\(\)**）

```js
// Class中的静态方法
        class Points {
            static classMethod() {
                return '这是静态方法'
            }
        }
        // 子类可以继承父类的静态方法
        class Son1 extends Points {}

        console.log(Son1.classMethod()) // => '这是静态方法'

        // 静态方法也可以从super对象上调用
        class Son2 extends Points {
            static sonClassMethod() {
                return super.classMethod() + '通过super继承父方法'
            }
        }
        console.log(Son2.sonClassMethod()) // => '这是静态方法通过super继承父方法'
```

---

## 14 Class的静态属性和实例属性

> es6 规定 Class内部只有静态方法，没有静态属性，因此只有下面写法正确

```js
class Dog {
        }
        Dog.leg = 4
        console.log(Dog.leg) // => 4

        // 下面写法都不对
        // class Cat {
        //     leg: 4
        //     static sy: '喵喵'
        // }
```

> 静态属性的提案

1 类的实例属性

```js
// 类的实例属性可以用等式，写入类的定义之中
        class Dog {
            // myProp = 42;

            constructor() {
                return this.myProp // 42
            }
        }
        let hsq = new Dog()
        console.log(hsq) // Dog {}

        // 之前的写法 constructor方法里面定义
        // class Alsj extends Dog {
        //     constructor() {
        //         super()
        //         this.state = {
        //             count: 1
        //         }
        //     }
        // }
        // const alsj = new Alsj()
        // console.log(alsj.state.count) // => 1

        // 通过=改写 这个写法浏览器目前报错 SyntaxError: Unexpected token =
        // class Alsj extends Dog {
        //     state = {
        //         count: 1
        //     }
        // }
        // const alsj = new Alsj()
        // console.log(alsj.state.count)

        // 为提高可读性，在constructor里面定义的实例属性，新方法可以直接列出
        class Alsj extends Dog {
            // state // => 目前浏览器SyntaxError: Unexpected identifier
            constructor() {
                super()
                this.state = {
                    count: 1
                }
            }
        }
        const alsj = new Alsj()
        console.log(alsj.state.count)
```

2 类的静态属性

```js
// 类的静态属性老方法
        class Dog {}
        Dog.leg = 4

        // 新写法
        class Cat {
            static leg = 4 // 直接写浏览器报错
        }
        const cat = new Cat()
        console.log(cat.leg)
```

---

## 15 new.target属性

> es6引入new.target属性，返回new命令作用于的那个构造函数，不是通过new调用的返回undefined

```js
// 使用方式1
        function Person(name) {
            if ( new.target !== undefined ) {
                this.name = name
            } else {
                throw new Error('实例化必须通过new生成')
            }
        }
        const person1 = new Person('Jeson')
        // const person2 = Person('Tom')
        console.log(person1) // => Person {name: "Jeson"}
        // console.log(person2) // => Uncaught Error: 实例化必须通过new生成

        // 使用方式2
        function People(name) {
            if ( new.target === People) {
                this.name = name
            } else {
                throw new Error('实例化必须通过new生成')
            }
        }
        const people1 = new People('Jack')
        // const people2 = People('sam')
        console.log(people1) // => Person {name: "Jeson"}
        // console.log(people2) // => Uncaught Error: 实例化必须通过new生成
```

> new.target返回当前的类，利用这个特点可以写出不能独立使用，必须继承后才能使用的类

```js
// 父类
        class Animale {
            constructor() {
                if ( new.target === Animale ) {
                    throw new Error('父类不能实例化')
                }
            }
        }
        class Dog extends Animale {
            constructor(name) {
                super();
            }
        }
        // const x = new Animale('狗') // 报错
        const y = new Dog('狗')
```



