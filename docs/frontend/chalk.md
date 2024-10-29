# chalk

粉笔，用粉笔画

npm i chalk
一个命令行修改文本颜色的库

想要比较轻量的库，用 yoctocolors

```js
import chalk from "chalk";

console.log(chalk.blue('hello world') + ' it is ' + chalk.red.bgWhite('beautiful!'))

const error = chalk.bold.red;
const warning = chalk.hex('#0f0')

console.log(error('test'))
console.log(warning('test'))
```