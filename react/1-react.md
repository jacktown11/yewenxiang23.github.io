---
title: React基础
---

### React环境搭建完毕后
在主入口文件 `index.js` 文件中引入 `react` 模块。

```
import React from "react";
import ReactDOM from "react-dom";
```

### React的基本语法

```
let age = 24;
let sex = 1;
let obj = {name:"hahaha"};
let add = () => 5+8;
let a = <div>
          <h1>叶文翔</h1>
          {/* 我是注释 */}
          <h1>年龄:{age+1}</h1>
          <h1>{obj.name}</h1>
          <h1 className={sex ? "a" : "b"}>{add()}</h1>
          <h1>性别:{sex ? "男" : "女"}</h1>
        </div>;

ReactDOM.render(
  a,document.getElementById("app")
)

```
- JSX语法，允许我们直接在JS里面去写标签。
- 每个标签必须关闭。
- 必须由一个标签来包裹，不然会报错
    - Module build failed: SyntaxError: Adjacent JSX elements must be wrapped in an enclosing tag (7:6)。
- jsx标签语句里面注释 `{/* something */}`。
- 我们可以在jsx元素内嵌入变量，方法，对象，等 {a}。
- class 写为 className，tabindex 为 tabIndex，for 写为htmlFor。
- JSX 语法会被编译，通过 `ReactDOM.createElement()` 这个方法。

### 组件component的3种方式
注意函数的首字母要大写，不然会报错。

```
//第一种方法 ES5的写法，现在基本没人用了
var Greeting = React.createClass({
    render: function(){
      return <h1>ye</h1>;
    }
})
//第二种方法常用在DOM结构的组合
function Greeting(){
  return <h1>hello</h1>;
}

//第三种
class Greeting extends React.Component {
  render(){
    return (
      <div>
        <h1>hello</h1>
      </div>
    )
  }
}

ReactDOM.render(
  <Greeting />,document.getElementById("app")
)
```
