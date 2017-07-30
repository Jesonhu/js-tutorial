## 1 Class通过关键字extends实现继承。ES5通过修改原型链实现继承

```js
// 父类
        class Animale {
            constructor(name, age) {
                this.name = name
                this.age = age
            }
            showAge() {
                return this.age
            }
            showName() {
                return this.name
            }
            toString() {
                return 'Animale的toString'
            }
        }

        // 继承
        class Dog extends Animale {
            constructor(name, age, ww) {
                super(name, age) // 调用父类Animale的constructor(name, age)
                this.ww = ww
            }
            toString() {
                return this.ww +' '+ super.toString() // 调用父类的toString
            }
        }
        const dog1 = new Dog('哈士奇', 1, '噢...')
        console.log(dog1.showAge()) // => 1
        console.log(dog1.showName()) // => '哈士奇'
        console.log(dog1.toString()) // => 噢... Animale的toString
```

> super关键字，用来表示父类的构造函数，用来新建父类的this对象， 子类必须在constructor方法中调用super方法，否则新建实例会报错，因为子类没有自己的this对象，而是继承父类的this对象，然后对其加工，不调用super\(\)方法，子类就得不到this对象

```js
class Animale {}
        // class Dog extends Animale {
        //     constructor() {
        //     }
        // }
        class Dog2 extends Animale {
            constructor() {
                super()
            }
        }

        // const dog1 = new Dog() // ReferenceError: this is not defined
        const dog2 = new Dog2()
```

> ES5的继承，实质是先创建子类的实例对象this, 然后再将父类的方法添加到this上面\(ParentClass.apply\(this\)\)。ES6是先创建父类的实例对象this（所以必须先super\(\)），然后在用子类的钩子函数修改this
>
> 如果子类没有constructor方法，这个方法会被默认添加。不管是否显式定义，任何一个子类都有constructor方法

```js
class Animale {
        }

        // class Dog extends Animale{}

        // 子类未显式指定constructor方法等同于
        class Dog extends Animale {
            constructor(...args) {
                super(...args)
            }
        }

        const dog1 = new Dog()
```

> 子类中只有调用super只后才能使用this

```js
class Animale {
            constructor(name, age) {
                this.name = name
                this.age = age
            }
   }
    class Dog extends Animale {
        constructor(name, age) {
            // this.name = name // ReferenceError: this is not defined
            super()
            this.name = age
        }
        showName() {
            return '这是一只狗'
        }
    }
    const dog1 = new Dog()
    console.log(dog1.showName())
```

---

## 2 Object.getPrototypeOf\(\) 方法可以在子类上获取父类

```js
class Animale {}
class Dog extends Animale {}
if ( Object.getPrototypeOf(Dog) === Animale ) {
    console.log(`Dog继承自Animale`)
}
```

## 3 super\(\)必须 且必须用在constructor方法里面

> super\(\)相当于 Parent.prototype.constructor.call\(this\) super虽然代表了父类的构造函数，但是**返回的是子类的实例**。即super内部的this指的是B
>
> 第二种情况：super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类、

```js
class Parent {
            p() { return 2 }
        }
class Son extends Parent {
    constructor() {
        super()
        console.log(super.p()) // => 2
    }
}
let son = new Son()
```

```js
// super指向父类的原型对象，所以定义在父类实例上的方法或属性，是无法通过super调用的。原型对象上的属性和方法可以获取到
        class Parent {
            constructor() {
                this.p = 2
            }
            getName() { return '父类方法' }
        }
        Parent.prototype.x = 666 // 定义在Parent.prototype super.x可以取得

        class Son extends Parent {
            get m() {
                return super.p
            }
        }
        const son = new Son()
        // p是父类Parent实例的属性，super.p就引用不到它
        console.log(son.m) // => undefined
        console.log(son.x) // => 666
        console.log(son.getName()) // => '父类方法'
```

> 如果super作为对象，在静态方法中。super指向父类。而不是父类的原型对象

```js
class Parent {
// static静态方法即构造函数对象上面的方法Parent.myMethod
    static myMethod(msg) {
        console.log('static', msg)
    }

    myMethod(msg) {
        console.log('instance', msg)
    }
}

class Son extends Parent {
static myMethod(msg) {
    super.myMethod(msg)
}

myMethod(msg) {
    super.myMethod(msg)
}
}

Son.myMethod(1) // => static 1
const son = new Son()
son.myMethod(2) // => instance 2
```

> super随执行环境的变化而变化
>
> 1 constructor 方法里面
>
> 2 静态方法里面
>
> 3 普通方法里面
>
> super使用的时候必须显示的指定是作为函数还是对象使用，否则会报错

---

## 4 类的\[prototype\]和\[\_\_proto\_\_\]

大多数浏览器的 ES5 实现之中，每一个对象都有`__proto__`属性，指向对应的构造函数的`prototype`属性。Class 作为构造函数的语法糖，同时有`prototype`属性和`__proto__`属性，因此同时存在两条继承链。

> （1）**子类的`__proto__`属性，表示构造函数的继承，总是指向父类。**
>
> （2）**子类`prototype`属性的`__proto__`属性，表示方法的继承，总是指向父类的`prototype`属性。**

```js
class Parent {}
class Son extends Parent {}

console.log( Son.__proto__ === Parent ) // => true
console.log( Son.prototype.__proto__ === Parent.prototype ) // => true

// 原理分析
class A {}
class B extends A {}

// B的实例继承A的实例
Object.setPrototypeOf(B.prototype, A.prototype)

// B的实例继承A的静态属性
Object.setPrototypeOf(B, A)

const b = new B()

// Object.setPrototypeOf()的实现
Object.setPrototypeOf = function(obj, proto) {
    obj.__proto__ = proto
    return obj
}
Object.setPrototypeOf(B.prototype, A.prototype)
//  等同于
B.prototype.__proto__ = A.prototype
Object.setPrototypeOf(B, A)
// 等同于
B.__proto__ = A
```

> 这两条继承链，可以这样理解：
>
> ```
>     作为一个对象，子类（B）的原型（\_\_proto\_\_属性）是父类（A）；
>
>     作为一个构造函数，子类（B）的原型（prototype属性）是父类的实例。
> ```

---

---

## extends的继承目标

> extends关键字后面可以跟多种类型的值。只要是一个有prototype属性的函数，就能被B继承。由于函数都有prototype属性（除了Function.prototype函数），因此A可以是任意函数。
>
> 子类继承Object类

```js
// 子类继承Object类
        class A extends Object {}

        console.log( A.__proto__ === Object ) // => true
        console.log( A.prototype.__proto__ === Object.prototype ) // => true

        // 此时A其实就是构造函数Object的复制，A的实例就是Object的实例
```

> 不任何继承
>
> 这种情况下，A作为一个基类（即不存在任何继承），就是一个普通函数，所以直接继承Function.prototype。但是，A调用后返回一个空对象（即Object实例），所以A.prototype.\_\_proto\_\_指向构造函数（Object）的prototype属性。

```js
class A {}
console.log( A.__proto__ === Function.prototype ) // => true
console.log( A.prototype.__proto__ === Object.prototype ) // => true
```

> 子类继承null
>
> 这种情况与第二种情况非常像。A也是一个普通函数，所以直接继承Function.prototype。但是，A调用后返回的对象不继承任何方法，所以它的\_\_proto\_\_指向Function.prototype，即实质上执行了下面的代码。

```js
class A extends null {}

console.log( A.__proto__ === Function.prototype ) // => true
console.log( A.prototype.__proto__ === undefined ) // => true

class A extends null {
    // 实际执行了这里的代码
    constructor() {
        return Object.create(null)
    }
}
// A也是一个普通的函数，所以直接继承Function.prototype
console.log( A.__proto__ === Function.prototype ) // => true
console.log( A.prototype.__proto__ === undefined ) // => true
```



