## 1 场景判断滑动的方向

touchmove的最后坐标减去touchstart的起始坐标。

1. X的结果如果正数，则说明手指是从左往右划动；
2. X的结果如果负数，则说明手指是从右往左划动；
3. Y的结果如果正数，则说明手指是从上往下划动；
4. Y的结果如果负数，则说明手指是从下往上划动。

---

## 2 原理

1 当开始一个touchstart事件的时候，获取此刻手指的横坐标startX和纵坐标startY；

2 当触发touchmove事件时，在获取此时手指的横坐标moveEndX和纵坐标moveEndY;最后，通过这两次获取的坐标差值来判断手指在手机屏幕上的滑动方向。

---

## 3 具体代码

```js
$("body").on("touchstart", function(e) {
    e.preventDefault();
    startX = e.originalEvent.changedTouches[0].pageX,
    startY = e.originalEvent.changedTouches[0].pageY;
});
$("body").on("touchmove", function(e) {
    e.preventDefault();
    moveEndX = e.originalEvent.changedTouches[0].pageX,
    moveEndY = e.originalEvent.changedTouches[0].pageY,
    X = moveEndX - startX,
    Y = moveEndY - startY;
  
    if ( X > 0 ) {
        alert("left 2 right");
    }
    else if ( X < 0 ) {
        alert("right 2 left");
    }
    else if ( Y > 0) {
        alert("top 2 bottom");
    }
    else if ( Y < 0 ) {
        alert("bottom 2 top");
    }
    else{
        alert("just touch");
    }
});
```

## 4 问题说明

在实际的操作中，手指的上下滑动很难做到直上直下，只要稍微有点斜，只要稍微有点斜，就会被X轴的判断先行接管，而与我们实际的操作意愿相背离。此时就需要添加特殊的判断技巧，代码如下：

```js
$("body").on("touchstart", function(e) {
    e.preventDefault();
    startX = e.originalEvent.changedTouches[0].pageX,
    startY = e.originalEvent.changedTouches[0].pageY;
});
$("body").on("touchmove", function(e) {
    e.preventDefault();
    moveEndX = e.originalEvent.changedTouches[0].pageX,
    moveEndY = e.originalEvent.changedTouches[0].pageY,
    X = moveEndX - startX,
    Y = moveEndY - startY;
      
    if ( Math.abs(X) > Math.abs(Y) && X > 0 ) {
        alert("left 2 right");
    }
    else if ( Math.abs(X) > Math.abs(Y) && X < 0 ) {
        alert("right 2 left");
    }
    else if ( Math.abs(Y) > Math.abs(X) && Y > 0) {
        alert("top 2 bottom");
    }
    else if ( Math.abs(Y) > Math.abs(X) && Y < 0 ) {
        alert("bottom 2 top");
    }
    else{
        alert("just touch");
    }
});
```

## 5 优化

$\('body'\).height = 0（此处是个大坑，但有时也不会出现，望大神指教）

故还应该在此基础上添加一下代码：

```js
var windowHeight = $(window).height(),
          $body = $("body");
      // console.log($(window).height());
      // console.log($('body').height());
      $body.css("height", windowHeight);
```

## 6 另一种实现

```html
<!doctype html>
<html lang="en">
 
    <head>
        <meta charset="UTF-8" />
        <title>Document</title>
        <meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no" />
        <script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
        <style type="text/css">
            * {
                padding: 0px;
                margin: 0px;
            }
             
            body {
                height: 100%;
                width: 100%;
                background: green;
            }
             
            .js {
                width: 300px;
                height: 300px;
                border: 1px solid red;
                margin: 0px auto;
            }
        </style>
        <script type="text/javascript">
            $(function() {
                $('body').on('touchstart', '.js', function(e) {
                    var touch = e.originalEvent;
                    startX = e.originalEvent.changedTouches[0].pageX;
                    startY = e.originalEvent.changedTouches[0].pageY;
                    $(".js").on('touchmove', function(e) {
                        e.preventDefault();
                        touch = e.originalEvent.touches[0] || e.originalEvent.changedTouches[0];
                        if(touch.pageX - startX > 10) {
                            console.log("右划");
                            $(".js").off('touchmove');
 
                        } else if(touch.pageX - startX < -10) {
                            console.log("左划");
                            $(".js").off('touchmove');
                        };
                        if(touch.pageY - startY > 10) {
                            console.log("下划");
                        } else if(touch.pageY - startY < -10) {
                            console.log("上划");
                        };
                    });
                    return false;
                }).on('touchend', function() {
                    $(".js").off('touchmove');
                });
            })
        </script>
    </head>
 
    <body>
        <div class="js">
 
        </div>
    </body>
 
</html>
```



