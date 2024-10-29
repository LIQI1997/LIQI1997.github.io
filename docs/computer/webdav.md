# webdav


WebDAV 是一个基于 HTTP 协议的协议，用来共享文件。

通过 http 协议来管理服务器上的文件，查询文件和文件夹内容，创建新文件和文件夹，修改文件内容，删除文件

可以自己搭建 webdav 服务端，也可以用123pan搭建好的 webdav 服务

webdav 实现原理：通过 http 协议去管理服务器上的文件

npm i webdav

```js
import { createClient } from 'webdav'

const client = createClient('https:/hostname:port', {
  username: 'xxx',
  password: 'xxx',
})

// client.getDirectoryContents('/').then(res => {
//   console.log(res)
// }).catch(err => {
//   console.log('err', err)
// })

client.getFileContents('/test.html', { format: 'text' }).then(res => {
  console.log('test js content', res)

  saveContent(res + '666 李其').then(res => {
    console.log('save res', res)
  }).catch(err => {
    console.log('err', err)
  })
}).catch(err => {
  console.log('err', err)
})

function saveContent(content) {
  return client.putFileContents('/test.html', content)
}
```