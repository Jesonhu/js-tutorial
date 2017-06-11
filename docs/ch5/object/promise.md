## 1 Promise的含义

Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6将其写进了语言标准，统一了用法，原生提供了`Promise`对象。

所谓`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

`Promise`对象有以下两个特点。

（1）对象的状态不受外界影响。`Promise`对象代表一个异步操作，有三种状态：`Pending`（进行中）、`Resolved`（已完成，又称 Fulfilled）和`Rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是`Promise`这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从`Pending`变为`Resolved`和从`Pending`变为`Rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果。如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

有了`Promise`对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，`Promise`对象提供统一的接口，使得控制异步操作更加容易。

`Promise`也有一些缺点。首先，无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。第三，当处于`Pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

如果某些事件不断地反复发生，一般来说，使用[Stream](https://nodejs.org/api/stream.html)模式是比部署`Promise`更好的选择。

---

## 2 基本用法

ES6 规定，`Promise`对象是一个构造函数，用来生成`Promise`实例。

```js
var promise = new Promise(function(resolve, reject){
    // 异步操作
    
    if (/* 异步操作成功 */){
      resolve(value);
    } else {
      reject(error);
    }
})
```

`Promise`构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

`resolve`函数的作用是，将`Promise`对象的状态从“未完成”变为“成功”（即从 Pending 变为 Resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；`reject`函数的作用是，将`Promise`对象的状态从“未完成”变为“失败”（即从 Pending 变为 Rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。



`Promise`实例生成以后，可以用`then`方法分别指定`Resolved`状态和`Reject`状态的回调函数。

```js
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

`then`方法可以接受两个回调函数作为参数。第一个回调函数是`Promise`对象的状态变为`Resolved`

时调用，第二个回调函数是`Promise`对象的状态变为`Rejected`时调用。

其中，第二个函数是可选的，不一定要提供。这两个函数都接受`Promise`对象传出的值作为参数。

Promise对象的简单例子

```js
// 实例化 Promise对象
let time = function(time) {
    return new Promise((resolve, reject) => {
       console.log('Promise实例化完成'); // Promise实例化后立即执行
       if (time >= 3000) {
           setTimeout(function(){
               resolve('大于3秒的时间后才显示出来');
           }, time)
       } else {
           reject('时间不能小于3秒')
       }
    });
};

// 实例化Promise对象后才能使用 then
time(1000).then((value) => { // 1000 会走第二条判断Reject(Pending => Reject 失败会走第二个回调)
    console.log(value);
}, (error) => {
    console.log(error);
});
console.log(1); // 'Promise实例化完成' 1 '时间不能小于3秒'
```

---

异步加载图片的例子

```js
function loadImageAsync(url) {
  return new Promise(function(resolve, reject) {
    var image = new Image();

    image.onload = function() {
      resolve(image);
    };

    image.onerror = function() {
      reject(new Error('Could not load image at ' + url));
    };

    image.src = url;
  });
}
```



