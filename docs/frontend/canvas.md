# Canvas

使用 Canvas 制作一个画板程序

Canvas 不能用 style 设置宽度和高度，会失真

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            margin: 0;
        }
        canvas {
            background-color: #f0f0f0;
        }
    </style>
</head>
<body>

    <canvas id="canvas" ></canvas>

    <script>
        const canvas = document.getElementById('canvas')

        canvas.height = window.innerHeight
        canvas.width = window.innerWidth

        const context = canvas.getContext('2d')
        context.strokeStyle = '#ff0000'
        context.lineWidth = 4

        let isDrawing = false;

        canvas.addEventListener("touchstart", e => {
            isDrawing = true;
            const x = e.touches[0].clientX;
            const y = e.touches[0].clientY;
            context.beginPath();
            context.lineTo(x, y)
            context.stroke()
            console.log('touchstart')
        })

        canvas.addEventListener("touchmove", e => {
            if (isDrawing) {
                const x = e.touches[0].clientX;
                const y = e.touches[0].clientY;
                context.lineTo(x, y)
                context.stroke()
                console.log('touchmove')
            }
        })
        
        canvas.addEventListener("touchend", (e) => {
            isDrawing = false;
        })
    </script>
</body>
</html>
```