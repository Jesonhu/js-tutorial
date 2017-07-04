## 1 简介

[HTML5](http://lib.csdn.net/base/html5)中新添加了很多事件，但是由于他们的兼容问题不是很理想，应用实战性不是太强，所以在这里基本省略，咱们只分享应用广泛兼容不错的事件，日后随着兼容情况提升以后再陆续添加分享。今天为大家介绍的事件主要是触摸事件：touchstart、touchmove和touchend。

　　一开始触摸事件touchstart、touchmove和touchend是[iOS](http://lib.csdn.net/base/ios)版Safari[浏览器](http://www.2cto.com/os/liulanqi/)为了向开发人员传达一些信息新添加的事件。因为[ios](http://lib.csdn.net/base/ios)设备既没有鼠标也没有键盘，所以在为移动Safari浏览器开发交互性网页的时候，PC端的鼠标和键盘事件是不够用的。

　　在iPhone 3Gs发布的时候，其自带的移动Safari浏览器就提供了一些与触摸\(touch\)操作相关的新事件。随后，[Android](http://lib.csdn.net/base/android)上的浏览器也实现了相同的事件。触摸事件\(touch\)会在用户手指放在屏幕上面的时候、在屏幕上滑动的时候或者是从屏幕上移开的时候出发。下面具体说明

## 2 事件介绍

**touchstart事件**：当手指触摸屏幕时候触发，即使已经有一个手指放在屏幕上也会触发。

**touchmove事件**：当手指在屏幕上滑动的时候连续地触发。在这个事件发生期间，调用preventDefault\(\)事件可以阻止滚动。

**touchend事件**：当手指从屏幕上离开的时候触发。

**touchcancel事件**：当系统停止跟踪触摸的时候触发。关于这个事件的确切出发时间，文档中并没有具体说明，咱们只能去猜测了。

上面的这些事件都会冒泡，也都可以取消。虽然这些触摸事件没有在DOM规范中定义，但是它们却是以兼容DOM的方式实现的。所以，每个触摸事件的event对象都提供了在鼠标事件中常见的属性：

bubbles\(起泡事件的类型\)、

cancelable\(是否用 preventDefault\(\) 方法可以取消与事件关联的默认动作\)、

clientX\(返回当事件被触发时，鼠标指针的水平坐标\)、

clientY\(返回当事件触发时，鼠标指针的垂直坐标\)、

screenX\(当某个事件被触发时，鼠标指针的水平坐标\)和

screenY\(返回当某个事件被触发时，鼠标指针的垂直坐标\)。

除了常见的DOM属性，触摸事件还包含下面三个用于跟踪触摸的属性

**touches**：表示当前跟踪的触摸操作的touch对象的数组。

**targetTouches**：特定于事件目标的Touch对象的数组。

**changeTouches**：表示自上次触摸以来发生了什么改变的Touch对象的数组。



每个Touch对象包含的属性如下。

**clientX**：触摸目标在视口中的x坐标。

**clientY**：触摸目标在视口中的y坐标。

**identifier**：标识触摸的唯一ID。

**pageX**：触摸目标在页面中的x坐标。

**pageY**：触摸目标在页面中的y坐标。

**screenX**：触摸目标在屏幕中的x坐标。

**screenY**：触摸目标在屏幕中的y坐标。

**target**：触目的DOM节点目标。

