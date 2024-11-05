# CSS


## 判断用户的浏览器是否是手机

- 可以用`navigator.userAgent`是否包含 Android 或 iPhone，但 userAgent 可以被用户修改
- 可以用 window.screen.width < 500，可如果用户手机横屏，width 可能超过 500px
- window.orientation 手机横屏时 === 90，竖屏时 === 0，pc 端为 undefined
-    alert(window.document.documentElement.ontouchstart) pc 端为 undefined

- let isMobile = window.matchMedia("only screen and (max-width: 760px)").matches; 这种方式本质上跟 screen.width < 500 类似，只不过是用 js 判断 css 规则是否匹配而已。

overflow-wrap:  break-word; 
normal 单词不会换行，只在单词之间的空格处换行
break- word 单词会换行，css 智能计算容器的宽度


.title1 + .title2 { } 紧邻兄弟组合器


## pointer-events

定义元素对鼠标指针事件是否做出反应，设置为 none，无法触发点击事件，hover 也没有效果

```css
        #btn {
            pointer-events: none;
            pointer-events: auto;
        }
```



## less
```less
@red: red;

@border: ~'1px solid #f0f';

button {
    border: @border;
    color: @red;

    &::after {
        content: ' >>>';
    }
}

@selector: ~'card';

// 将转义的字符串用作选择器
.@{selector} {
    height: 400px;
    width: 400px;
    border: 2px dashed green;

    &-title {
        color: @red;
    }
}


@import "./variables.less";

body {
    color: @color;
}

// mixins
.border {
    border: 2px dotted green;
    border-radius: 24px;
    padding: 24px;
}

.card {
    .border();
}


// nesting
.card {
    .header {
        .title {
            font-weight: bold;
            font-size: 2em;
            &::after {
                content: '*';
            }
        }
    }
    &-footer {
        height: 2px;
        background-color: blue;
    }
}

// 规则嵌套和冒泡
.card {
    background-color: gold;
    @media (min-width: 768px) {
        background-color: black;
    }
}

// 字符串转义 escaping
@class-name: ~'card';

.@{class-name} {
    font-style: italic;
}
```

## font

ttf/woff/woff2/eot/otf


## dialog

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .mask {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      background-color: #999;
      animation-name: opacity;
      animation-duration: .4s;
      animation-timing-function: linear;
    }

    @keyframes opacity {
      0% {
        opacity: 0;
      }
      100% {
        opacity: 1;
      }
    }

    .dialog {
      background-color: #fff;
      border-radius: 8px;
      padding: 24px;
      animation-name: scale;
      animation-duration: .2s;
      animation-timing-function: linear;
    }

    @keyframes scale {
      0% {
        transform: scale(0);
      }
      100% {
        transform: scale(1);
      }
    }
  </style>
</head>
<body>
  <button id="btn">open</button>

  <script>
    document.getElementById('btn').addEventListener('click', () => {
      const mask = document.createElement('div')
      mask.innerHTML = `
        <div class="mask">
          <div class="dialog">
            dialog
          </div>
        </div>
      `
      document.body.appendChild(mask)
    })
  </script>

</body>
</html>
```