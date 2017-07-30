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



