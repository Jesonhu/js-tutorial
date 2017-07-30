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

---

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

## 13 Class的静态方法

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上`static`关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。（**构造函数自有的方法，例如A.sayName\(\)**）

