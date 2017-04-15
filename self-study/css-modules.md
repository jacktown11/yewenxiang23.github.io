---
title: CSS Modules 用法
---

### 示例demo

```bash
$ git clone https://github.com/ruanyf/css-modules-demos.git
```

### 局部作用域

CSS的规则都是全局的，任何一个组件的样式规则，都对整个页面。

产生局部作用域的唯一方式，就是使用一个独一无二的class的名字，不会与其他选择器重名，这就是CSS Modules的做法

### 使用webpack配置CSS Modules方法

CSS Modules 提供各种[插件](https://github.com/css-modules/css-modules),支持不同的构建工具。 webpack 的 css-loader插件对 CSS Modules 的支持最好。

```js
{
  test: /\.css$/,
  loader: "style-loader!css-loader?modules"
}
```
关键的一行是style-loader!css-loader?modules，它在css-loader后面加了一个查询参数modules，表示打开 CSS Modules 功能。

#### 使用方式

css样式

```css
.title {
  color: red;
}
```

```
import React from 'react';
import style from './App.css';

export default () => {
  return (
    <h1 className={style.title}>
      Hello World
    </h1>
  );
};
```
上面代码中，我们将样式文件App.css输入到style对象，然后引用style.title代表一个class。
构建工具会将类名style.title编译成一个哈希字符串。这样一来，这个类名就变成独一无二了，只对App组件有效。

### CSS Modules 全局写法

```css
:global(.title) {
  color: green;
}
```

引用的方式
```
import React from 'react';
import styles from './App.css';

export default () => {
  return (
    <h1 className="title">
      Hello World
    </h1>
  );
};
```
CSS Modules 还提供一种显式的局部作用域语法:local(.className)，等同于.className，所以上面的App.css也可以写成下面这样。

### 定制哈希类名

css-loader默认的哈希算法是[hash:base64]，这会将.title编译成 `._3zyde4l1yATCOkgn-DBWEL` 这样的字符串。

`webpack.config.js`里面可以定制哈希字符串的格式

```
module: {
  loaders: [
    // ...
    {
      test: /\.css$/,
      loader: "style-loader!css-loader?modules&localIdentName=[path][name]---[local]---[hash:base64:5]"
    },
  ]
}
```

会发现.title被编译成了`demo03-components-App---title---GpMto`。

### Class 的组合

在 CSS Modules 中，一个选择器可以继承另一个选择器的规则，这称为 `组合(composition)`

例如 让.title继承.classname

```
.classname{
  background-color:blue;
}
.title{
  composes:classname;
  color:red;
}

```

使用样式正常引用 `style.title` 就行了，编译后 `<h1 class="_2DHwuiHWMnKTOYG45T0x34 _10B-buq6_BEOTOl9urIjf8">`。

### 继承其他CSS文件的规则

```css
.title {
  composes: className from './another.css';
  color: red;
}
```

### 输入变量

CSS Modules 支持使用变量，不过需要安装 postcss-loader 和 postcss-modules-values

```bash
$ npm install --save postcss-loader postcss-modules-values
```

把 `postcss-loader` 加入 webpack.config.js 文件中

```
module.exports = {
  entry: __dirname + '/index.js',
  output: {
    publicPath: '/',
    filename: './bundle.js'
  },
  module: {
    loaders: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        loader: 'babel',
        query: {
          presets: ['es2015', 'stage-0', 'react']
        }
      },
      {
        test: /\.css$/,
        loader: "style-loader!css-loader?modules!postcss-loader"
      },
    ]
  },
  postcss: [
    values
  ]
};
```

接着在 colors.css 里面定义变量

```
@value blue: #0c77f8;
@value red: #ff0000;
@value green: #aaf200;
```

在其他css文件中引入变量

```
@value colors: "./colors.css";
@value blue, red, green from colors;

.title {
  color: red;
  background-color: blue;
}
```
