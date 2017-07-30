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



