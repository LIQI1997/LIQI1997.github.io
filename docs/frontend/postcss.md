# PostCSS

PostCSS 类似于 CSS 的 Babel，允许 js 来修改 css 代码，具体的功能通过不同的插件来实现，比如压缩css代码，支持 css 嵌套，css 选择器命名重置等等。


把 CSS 源代码解析成 AST，处理后，再把 AST 处理成字符串

有哪些功能？

识别浏览器不支持的 CSS 新特性，加上浏览器私有前缀

识别冗余代码，去除并压缩 CSS 代码，用的是 cssnano