# Node.js 日志库

## winston

```js
const winston = require('winston')

const logger = winston.createLogger({
  transports: [
    new winston.transports.Console(),
    new winston.transports.File({
      filename: 'test.log'
    })
  ],
  format: winston.format.combine(
    winston.format.label({
      label: 'test label'
    }),
    winston.format.timestamp(),
    winston.format.prettyPrint()
  )
})

logger.info('hello')

logger.error('error')
```