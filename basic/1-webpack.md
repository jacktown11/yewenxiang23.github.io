---
title: webpack环境搭建和配置
---

node.js的模块分为3类：

  - 核心模块: 是node.js自带的模块，后台需要用到它，目前不用管。

  - 第三方模块: 是通过 `npm install <Module Name> --save-div` 安装的包，包在 `node_modules` 文件夹中。

  - 自定义模块: 是自己写的JS代码模块文件。

### 首先在初始化项目文件
```
npm init
//在项目文件夹中生成一个package.json文件
```

现在建立三个文件，分别是 `index.html` `index.js` `text.js` 正常情况下一个JS文件是无法引用另外一个JS文件的，但是通过node.js环境 和webpack环境就能引用另外一个js文件。

```js
var a = require("./test");
a();
//index.js中的代码
```

```js
function a(){
  console.log("我是一个函数");
}
module.exports = a;
//test.js中的代码
```

在 `index.html` 中引入 `index.js` 执行在浏览器中会报错,因为正常情况下一个JS文件是无法引入另外一个JS文件的。在node.js环境中JS就可以引用另外一个JS文件，在命令行中输入 `node index.js` 输出 *“我是一个函数”*.这步操作与 webpack包无关,也就是不装webpack包也能输出 *“我是一个函数”* 。

### 装webpack包

`webpack` 是一个前端资源模块化管理和打包工具。它可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分隔，等到实际需要的时候再异步加载。通过 loader 的转换，任何形式的资源都可以视作模块，比如 CommonJs 模块、 AMD 模块、 ES6 模块、CSS、图片、 JSON、Coffeescript、 LESS 等。

```
npm install webpack --save-dev
```
>由于webpack是一个工具 所以加上-dev参数
>

### 装好之后如何使用 webpack命令呢？

```bash
./node_modules/.bin/webpack --help
直接输入webpack是不行的，webpack命令在 `./node_modules/.bin/` 文件夹中输才行
```

上面的代码能查看webpack全部的命令，但是每次这么输入是非常麻烦的，我们在 `package.json` 文件 `script` 字段中输入脚本：

```json
"build":"./node_modules/.bin/webpack --help"
```
然后在命令行中输入：

```bash
npm run build
```

现在 `npm run build` 与 `./node_modules/.bin/webpack --help` 两条bash命令是等价的，`build` 为定义的名字，可以随便取。

### 把两个js文件打包成一个JS文件

可以通过以下方式：

```json
"build":"./node_modules/.bin/webpack index.js yewenxiang.js"

首先package.json 文件中script字段做出如上改变
```

```bash
npm run build
然后命令行输入这个代码
```
就会在项目文件夹中生成一个 `yewenxiang.js` 文件，这个文件是 `index.js` 和 `test.js` 文件的合体。webpack包把这两个文件打包在一起，而且相同的全局变量不会导致冲突。就可以在 `index.html` 文件中引入 `yewenxiang.js` 这个文件了，浏览器就不会报错了。

### 更好的方式
增加webpack包的配置文件 `webpack.config.js` 里面添加如下内容

```js
module.exports = {
  entry: './index.js',
  output: {
    filename: 'bundle.js'
  }
}
```
 `entry` 是入口的意思，`output` 是出口的意思。

```bash
./node_modules/.bin/webpack
package.json script字段如上改变
```
然后 bash 命令输入 `npm run build` 一样可以达到打包的效果，一般是通过这种方式来实现，当执行build脚本时，会去读取 `webpack.config.js` 里面的配置信息。

### 杂记

```js
// 通过 module.exports 导出模块，require 导入模块。

// 导入核心模块
var fs = require('fs')

// 导入第三方模块
var $ = require('jquery')

// 导入自定义的模块
var test = require('./test')

// 导出模块
module.exports.test = 'test';
```
