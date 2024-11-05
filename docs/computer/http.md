# HTTP


## 1.0

http1.0 一次 tcp 连接只能发送一次请求，发送完毕就关闭 tcp 连接了。

tcp 连接成本很高，要客户端和服务端三次握手，并且开始时发送速度慢 slow start

为了解决这个问题，有一个非标准字段 Connection: keep-alive，请求时带上，服务端也返回，这样服务器就不会关闭 tcp 连接，其他的请求就能复用。

## 1.1

### 增加了 持久连接 

persistent connection，即 tcp 默认不关闭，多个请求复用一个 tcp 连接，不用声明 Connection: keep-alive

客户端和服务器过了一段时间发现对方没有活动了，就会主动关闭连接，规范的做饭是，浏览器最后一个 http request，header 中有 Connection: close，要求服务器关闭 tcp 连接。

同一个域名，大多数浏览器允许同时建立6个连接。

### 管道机制 pipelining
一个 tcp 连接中，浏览器可以同时发送 A 和 B 请求，但服务器还是按照顺序先返回 A 响应，再返回 B 响应。

Content-Length http response header 中的字段，表明这次响应的数据长度

也可以不设置 Content-Length，服务器采用流的形式返回数据，这样就不用等服务器计算好响应体的长度后才开始发送响应数据包。

响应的请求头中，有 Transfer-Encoding: chunked，表明这是流式（stream）。

最后一个块，只会返回一个十六进制的 0，表示流结束了。

1.1 虽然可以复用 tcp 连接，但响应还是按照服务端收到请求的顺序，一个一个来返回响应。所以如果前面的响应，服务端返回的慢，后面的请求想要的响应就得排队等着，这叫队头堵塞（head of line blocking）

为了解决这个问题，有很多优化技巧，比如减少请求数量，合并 css 和 js 代码文件，合并图片成精灵图，将图片编码成 base64，放在 html 或 css 中。域名分片（domain sharding）

SPDY 谷歌开发的，http2 的前身

2015年发布了 http2



### HTTP/2

1.1 http header 是 ascii 编码，也就是文本，http body 可以是二进制。
http2 header 和 body 都是二进制，叫做帧率 frame：头信息帧和数据帧

http2 复用了 tcp 连接，请求和响应都可以并行发送，而且不用按照顺序返回了，这样就避免了队头堵塞。

举个例子，客户端发送了 A 和 B 请求，服务器在处理 A 的过程中发现很耗时，于是返回了一部分，同时也在返回 B，而 A 处理有了结果，接着返回。

并且在服务器返回的过程中，客户端也可以接着请求，这种双向，实时通信，就叫 双工（multiplexing）多路复用。

因为客户端和服务端发送的数据包没有顺序了，所以客户端和服务端发送的每一个 http 包，都叫做一个 数据流 stream，每个数据流都有一个 stream id，唯一编号，客户端发送的 id 都是奇数，服务器响应的 id 都是偶数

数据流在发送的过程中，客户端和服务器都可以发送一个信号： RST_STREAM 帧，用来取消这个数据流。

1.1 想要取消请求或响应，只能关闭 tcp 连接。

2 取消了某个请求，tcp 连接还在，其他请求还能用。

客户端也能指定数据流的优先级，让服务器优先处理返回。


http 协议不带状态，所以有关数据每次请求和响应都会携带上，比如 cookie 和 user agent，这样浪费了很多带宽，也影响速度。

所以现在 http header 可以被压缩，用 gzip 或 compress 压缩后再发送。

客户端和服务器还共同维护了一张头信息表，所有字段都在表里，每个字段都有一个索引号，以后就不发送具体的数据了，只发送索引号即可，减少传输的字段，提高网站访问速度。


服务端推送（server push）？


### 虚拟主机

客户端请求头中还加了 host。

一台服务器，一个固定 IP，是可以搭建多个网站的，a.com:80, b.com:8080 只要占服务器的端口不重复，服务器能区分对应的服务程序即可。

因为 http 是应用层协议，在 tcp/ip 协议之上，所以客户端跟服务器通信，用的就是 ip 地址进行通信，所以 http request header 中有 host，就能够让一台服务器收到请求时，判断是哪一个服务程序，交给它处理。





## Content-Type

是用来指定 http request or response body data type

类型有两种：Media Type，

MIME：Multipurpose Internet Mail Extensions

格式是：type/subtype


Mpeg 音频格式


xhtml 格式
```js

application/atom+xml Atom Xml 聚合格式

application/msword word文档格式

application/octet-stream 二进制流数据，文件下载就是这个

application/x-www-form-urlencoded form 表单默认的格式，被编码成 key/value格式


multipart/form-data 表单中文件上传，就是这个格式

<form> 的 encType 属性可以设置这两种

```

Content-Type 厂商可以自定义