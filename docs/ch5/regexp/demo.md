##正则案例

#### 字符串合并多个空格(全局匹配)
```js
function formateSpace(str) {
  var regExp = /\s+/g;
  return str.replace(regExp, '');
}
console.log( formateSpace('ss   11   22') ); // 'ss1122'
```

***

#### 保留字符串中的数字
```js
function getNum(str){
  var regExp = /[^\d]/g; // 除了数字外的其他字符串
  return str.replace(regExp, '');
}
console.log( getNum('23ss11') ); // '2311'
```
***

#### 保留字符中的中文
```js
function getCN(str){
  var regExp = /[^\u4e00-\u9fa5\uf900-\ufa2d]/g; // 除了中文外的其他字符串
  return str.replace(regExp, '');
}
console.log( getCN('李四23ss11张三') ); // '李四张三'
```
****
#### 获取字节长度
```js
function getLen(str) {
            var regExp = /^[\u4e00-\u9fa5\uf900-\ufa2d]+$/;
            if (regExp.test(str)) { // 中文
                console.log('cn');
                return str.length*2;
            } else {
                var oMatches = str.match(/[\x00-\xff]/g); // 数组
                var oLength = str.length*2 - oMatches.length;
                return oLength;
            }
        }
        console.log( getLen('张三') ); // 4
        console.log(getLen('abc')); // 3
```
***
#### html标签转义
```js
/* 转义字符串 */
        function htmlEscape(str) {
                return str.replace(/[<>"&]/g, (item, index, originStr) => {
                     switch (item) {
                        case '<':
                            return '&lt;'; 
                        case '>':
                            return  '&gt;';
                        case '&':
                            return '&amp;';
                        case '\"':
                            return '&quot;';            
                     }   
                })
        };
        let str = "<p>123</p><br>"
        console.log( htmlEscape(str) ); // '&lt;p&gt;123&lt;/p&gt;&lt;br&gt;'
```


