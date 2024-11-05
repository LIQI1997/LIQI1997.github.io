# ico

```js
        function changeFavoriteIcon() {
            const link = document.querySelector('link[rel="icon"]')
            if (link) {
                document.head.removeChild(link)
            }
            const newIcon = document.createElement('link')

            newIcon.rel = 'icon'
            newIcon.type = 'image/png'
            newIcon.href = 'test.png'

            document.head.appendChild(newIcon)
        }
```