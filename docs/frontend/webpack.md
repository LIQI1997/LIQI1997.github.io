# webpack






copy-webpack-plugin
一般将静态资源从一个目录copy到另一个目录，还可以做转换

自定义脚手架的意义是封装很多功能，对外暴露函数出去，方便程序员开发时只需要写业务代码，不需要看官方的 api 了。
但自定义讲究的是命名规范，封装优雅，放在的位置合适，不能来回嵌套，这样的代码就不是好的脚手架。


grunt
gulp

webpack 动态打包所有依赖并创建 “依赖图”，用来编译 javascript 模块

--save
--save-dev
区别

npx webpack
npx 的作用，从当前目录中的 node_modules/.bin 中寻找可执行的二进制文件 webpack

scripts 中定义的命名
npm run build "build": "webpack" 
会直接从当前目录中的 node_modules 中寻找可执行的二进制文件






loader 和 plugin 的区别

loader 负责文件转换，比如加载不同模块的文件，既可以加载
commonJS 模块，又能加载 ES6 Module，加载 CSS 文件

多个loader按照配置文件中定义的顺序逆序执行，前一个loader会把执行结果（转换后的资源）传递给下一个loader
css-loader 负责读取css文件，转换成 js 字符串
style-loader 里面有个函数，功能是创建 style 标签，插入到 head 中

webpack 处理图片有三种，
第一种是 html 中直接指向的，html-loader 处理，如<img src="./test.png" />
第二种是 css 中指向的，css-loader 处理，background: url('./test.png');
第三种是 js 中动态设置的，webpack5 内置功能了，asset/resource
import test from './test.png'; 
document.getElementById('test').src = test;

本质上都是处理图片路径，能够找到这个图片而已，打包后就是把图片移动到 dist 资源包中，如果是小图片可以生成 base64 字符串，虽然增加了图片体积，但减少了额外的网络请求。

在 css 中使用自定义字体，css-loader 已经处理了，如果需要在 js 中处理 font 字体文件，那就需要用 asset/resource 了。

module: {
        rules: [
            {
                test: /\.css$/i,
                use: [
                    'style-loader', 'css-loader'
                ]
            },
            // {
            //     test: /\.(png|jpg|jpeg|svg|gif)$/i,
            //     type: 'asset/resource'
            // }
        ]
    }

webpack 内置 json 文件导入 import testData from 'test.json'
处理 csv 和 tsv 用 csv- loader
处理 xml 用 xml-loader
webpack 会把数据转换成结构化的 json 格式的数据

import { data } from 'test.json' 会有警告，规范不允许这样做，还待我研究

处理 toml，yaml，json5 格式，可以用自定义解析器，也跟其他loader一样，在 module.rule 中配置即可

sass-loader
less-loader
postcss

url-loader 负责将文件转换成 data: 字符串形式，用来加载
uglyfyjs-webpack-plugin，用来压缩 js 代码


thread-loader
使用时，需将此 loader 放置在其他 loader 之前。放置在此 loader 之后的 loader 会在一个独立的 worker 池中运行。

cache-loader
在一些性能开销较大的 loader 之前添加此 loader，以将结果缓存到磁盘里。

terser-webpack-plugin 压缩js，去除生产环境中的 console 和 debugger 等。

file-loader 处理 import test from 'a.jpg' 文件路径问题，最后打包后的结果就是可访问的路径。

extract-text-webpack-plugin 抽离 css，防止将样式打包在js 中，引起页面样式错乱。

copy-webpack-plugin 将一些静态文件拷贝到打包后的资源包中。

bundle-loader 用来动态加载不同的chunk，浏览器遇到会发起请求。



处理 html
如果是单入口，webpack 会分析代码中的模块，把多个相互依赖的 js 文件打包成一个 bundle，这时候手动维护html 文件，还比较简单，只需要添加一个 script 标签，src 指向即可。
如果是多入口，每个js文件都是不同的作用，那手动创建 html 文件，一个一个导入就很麻烦了

html-webpack-plugin 就会自动把所有的 bundle 都加载进去
plugins: [
        new HtmlWebpackPlugin({
            title: 'output'
        })
    ]

每次构建都清理一下 dist 目录
output: {
        filename: '[name].bundle.js',
        path: path.resolve(__dirname, 'dist'),
        clean: true,
    },

webpack 通过 manifest 追踪模块到输出的 bundle 之间的映射关系，webpack manifest plugin 可以把 manifest 提取成 json 文件 


开发环境 source map
浏览器执行的是 bundle 文件，里面有多个功能散落在多个模块文件中，发生了错误，调试很难，因为浏览器错误堆栈无法准确地定位到错误的代码在哪个文件哪一行，它只会指向 bundle 的某一行。

source map 可以将编译后的代码映射回原始源代码

inline- source-map，开发环境专用



使用 webpack 命令，每次改完代码，手动打包很麻烦，可以用 webpack --watch，缺点是还得手动刷新浏览器
使用 webpack-dev-server，自动刷新浏览器，应该使用 web socket 协议




代码分离
把所有的 js 模块打包成一个 bundle，然后再加载，如果 bundle 过大，加载速度会很慢，所以代码分离，可以让客户端浏览器加载代码的时候，首屏只加载首屏的代码，如果是单页应用，切换页面后再加载对应的页面组件的 js 文件。
更加细粒度的控制，那就是有些函数通过代码里使用 promise 动态导入，浏览器执行的时候，用户点了按钮，再请求对应的js文件，请求成功后再执行逻辑，虽然有点延迟，但也能提高最初的页面加载速度。

实现方式就是多入口打包，

模块热更替



babel-loader 将 ES6 代码转换成 ES5 代码

url-loader，file-loader 转换图片成 base64

plugin 负责 loader 无法完成的功能，扩展 webpack 的功能，

压缩代码，定义环境变量

commonChunkPlugin 负责提取第三方库和公共模块，避免
bundle 过大，
多页应用，创建各个页面公用的 bundle


用 node index.js 启动一个脚本

跟用 webpack-dev-server 加 entry 启动一个脚本，本质上没有区别，webpack-dev-server 多了一个编译的动作，可以帮你处理导入的模块，在你更新 js 代码时，自动重启这个脚本。


bundle-loader


webpack 的 define plugin，编译的时候定义常量，并且找到代码中使用到此常量的地方，全局替换掉，替换的结果不包括引号，所以建议加上引号

document.body.innerHTML =  SUM + HELLO + FLAG

plugins: [
        new HtmlWebpackPlugin({
            title: 'output 666'
        }),

        new DefinePlugin({
            SUM: '1+1', // 传的是表达式
            HELLO: JSON.stringify('hello'), // 这传的才是字符串
            FLAG: true, 
        })
    ]



externals: {
jquery: 'jquery',
lodash: 'commonjs2 lodash'
}

如果导入的库来源于 script src，cdn 链接，也就是由运行环境提供，不需要打包进 bundle里面，那就可以配置 externals，commonjs2 导出的是 module.exports.default
externals 暴露的是全局变量

new webpack.ExternalsPlugin("commonjs", ["ffi-napi"]),
    new webpack.EnvironmentPlugin({
      FLUENTFFMPEG_COV: false,
    }),
new webpack.EnvironmentPlugin({
      NODE_ENV: 'development',
    }),
    new WriteFilePlugin({
      test: (file) => !file.includes('hot-update'),
    }),



视觉稳定性指标 CLS，Cumulative Layout Shift



秒开率

报性能监控平台

snowpack
rust

powershell
nslookup 20.205.243.166
根据ip查域名

corepack nodejs 自带的工具，可以用来安装 yarn，pnpm

