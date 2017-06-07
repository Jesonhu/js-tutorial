##ExpReg方法

 用RegExp构造正则表达式比直接用正则表达式直接量的优势就在于可以动态地创建一个正则表达式
 ***
 ####3.1、exec()

　　　　RegExp对象的主要方法，专门用来捕获数组

　　　　返回结果为一个Array数组但是包含两个额外的属性 input和index

　　　　index: 匹配项在字符传中的位置

　　　　input: 应用正则表达式的字符串

　　　　该方法和String方法match()很相似，只不过它是以字符串为参数。如果没有匹配到，它将返回null。反之将返回一个数组。

　　　　这里的数组具体内容和非全局的match匹配一样。数组[0]是匹配到字符串，数组[n]是对应捕获分组。而且index和input也和match一致。
　　　　
　　　　
　　　　PS:无论正则表达式是否具有g标示，exec返回的数组类型都是一样的，但是它会把lastIndex属性设置到紧接匹配子串的字符位置，以便下次继续匹配，因为这点特殊性，exec可以遍历调用。
　　　　
```js
var reg = /abc/g;
var str = 'abc,abc';
var result;
var sign = 0;
while((result = reg.exec(str)) != null) {
    console.log(result[0], result.index, result.input, reg.lastIndex);
    if (sign == 0) {
        sign = 1;
        reg.lastIndex = 0;
    }
}
```
***
#### test()

　　　　这个方法较简单，就一个参数，如果匹配上了，就返回true，反之为false。它也支持遍历，就相当于调用exec返回值不是null，它就返回true。

　　　　PS:这个要小心一个坑，如果你用这2个方法匹配多个字符串，而每次匹配一个字符串又没有匹配完时，lastIndex属性不会自己重置为0的。下面是例子。
　　　
```js
var reg = /abc/g;
var str = 'abc,12abc';
var result;
result = reg.exec(str);
console.log(result[0], result.index, result.input, reg.lastIndex);

str = 'abc,345abc';
result = reg.exec(str);
console.log(result[0], result.index, result.input, reg.lastIndex);
```
***
####compile()

可用于改变和重新编译正则表达式。当在循环中有重复正则匹配的时候，用编译后的正则表达式执行起来，效率更高（前提是正则表达式是固定的，如果你每次循环都需要动态适配新正则的话，是没有效果的）。
