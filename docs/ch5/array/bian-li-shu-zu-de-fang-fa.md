## 1 最原始的 for...

```
for (let i = 0; i < length; i++){}
```

## 2 forEach 无法中途跳出循环，break和return命名都不能奏效

```js
myArray.forEach(function() {})
```

## 3 for...in

for…in主要是为遍历对象而设计的，不适用于遍历数组。 

遍历数组的缺点： 

1. 数组的键名是数字，但是for…in循环是以字符串作为键名“0”、“1”、“2”等等。 

2. for…in循环不仅遍历数字键名，还会遍历手动添加的其他键，甚至包括原型链上的键。 

3. 某些情况下，for…in循环会以任意顺序遍历键名。

## 4 for...of ES6新增

```js
for(let value of myArray){
    console.log(value);
}
```

优点： 

1. 有着同for…in一样的简洁语法，但是没有for…in那些缺点。 

2. 不同用于forEach方法，它可以与break、continue和return配合使用。 

3. 提供了遍历所有数据结构的统一操作接口。

