## 1 语法

```js
element.addEventListener(event, function, useCapture)
```

#### 参数说明

| event | 必须,字符串 事件名,不需要前缀on |
| :--- | :--- |
| function | 必须。指定要事件触发时执行的函数。                                     当事件对象会作为第一个参数传入函数。 事件对象的类型取决于特定的事件。例如， "click" 事件属于 MouseEvent\(鼠标事件\) 对象。 |
| useCapture | 可选。布尔值，指定事件是否在捕获或冒泡阶段执行。             true - 事件句柄在捕获阶段执行                                                    false- false- 默认。事件句柄在冒泡阶段执行 |

## 2 移除事件

使用 removeEventListener\(\) 方法来移除 addEventListener\(\) 方法添加的事件句柄。

**注意：**

Internet Explorer 8 及更早IE版本不支持 addEventListener\(\) 方法，，Opera 7.0 及 Opera 更早版本也不支持。 但是，对于这些不支持该函数的浏览器，你可以使用

**attachEvent\(\)**

方法来添加事件句柄 \(查看 "更多实例" 了解跨浏览器的解决方案\)

```js
var x = document.getElementById("myBtn");
if (x.addEventListener) {                    //所有主流浏览器，除了 IE 8 及更早 IE版本
    x.addEventListener("click", myFunction);
} else if (x.attachEvent) {                  // IE 8 及更早 IE 版本
    x.attachEvent("onclick", myFunction);
}
```



