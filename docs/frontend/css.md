# CSS

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