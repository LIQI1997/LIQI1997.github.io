# marquee

使用 CSS 实现了跑马灯的效果，鼠标悬浮上去还能暂停动画。

```html
<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .container {
      width: 500px;
      background-color: #999;
      margin: 20px auto;
      overflow-x: hidden;
      white-space: nowrap;
    }
    .marquee {
      color: #fff;
      display: inline-block;
      padding-left: 100%;
      animation-name: move;
      animation-duration: 4s;
      animation-timing-function: linear;
      animation-iteration-count: infinite;
    }

    .container:hover .marquee {
      animation-play-state: paused;
    }

    @keyframes move {
      0% {
        transform: translateX(0);
      }
      100% {
        transform: translateX(-100%);
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="marquee">Hello world</div>
  </div>
</body>
</html>
```