# webdav


WebDAV 是一个基于 HTTP 协议的协议，用来管理服务器上的文件，如查询文件和文件夹内容，创建文件和文件夹，修改文件内容，删除文件等等。

可以自己搭建 webdav 服务端。

```bash
npm i webdav
```

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
  console.log('res', res)

  saveContent(res + '666').then(res => {
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