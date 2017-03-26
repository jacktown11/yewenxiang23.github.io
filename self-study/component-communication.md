---
title: 组件之间互相通信总结
---

# props

父组件给子组件设置 `props`,然后子组件就可以通过 this.props 来获取到父组件的 **数据** 和 **方法** 。

### 给子组件传函数，实现子组件调用父组件的方法

```js
//父组件
class App extends Component {
  constructor(){
    super();
    this.state={
      text:"我是初始文本"
    }
  }
  handleClick(){
    this.setState({text:"我的文本被改改变了"})
  }
  render() {
    return (
      <div className="App">
        {this.state.text}
        <MyComponent fuc={this.handleClick.bind(this)}/>
      </div>
    );
  }
}
```

```js
//子组件
class MyComponent extends Component {
  render() {
    return (
      <div>
      <input type="button" value="子组件按钮" onClick={()=>this.props.fuc()}></input>
      </div>
    );
  }
}
```
点击子组件按钮，父组件的内容被改变了

# ref

ref 可以用来绑定到 render() 输出的任何组件上

- ref:绑定属性
- refs:调用的时候使用

### ref可以调用子组件方法

```js
//父组件
class App extends Component {
  handleClick(){
    this.refs.subcomponents.subHandleClick();
  }
  render() {
    return (
      <div className="App">
        <input type="button" value="点我调用子组件的方法" onClick={this.handleClick.bind(this)}/>
        <MyComponent ref="subcomponents" name="yewenxiang"/>
      </div>
    );
  }
}
```

```js
//子组件
class MyComponent extends Component {
  constructor(props){
    super(props);
    this.state={
      text:'这是初始文本'
    }
  }
  subHandleClick(){
    this.setState({text:'文本被改变了，哈哈'})
  }
  render() {
    return (
      <div>
        查看:{this.state.text}
      </div>
    );
  }
}
```

### ref可以获取DOM节点

```js
class MyComponent extends Component {
  handleClick(){
    console.log(this.refs.text)
  }
  render() {
    return (
      <div ref="text" onClick={this.handleClick.bind(this)}>点击获取自身节点</div>
    );
  }
}
```
上面代码点击之后，获取了div这个节点，常见的用法有获取 input输入的value值。比如下面代码

```js
class MyComponent extends Component {
  handleSubmitClick = () => {
    const name = this._name.value;
    console.log(name)
  }
  render() {
    return (
      <div>
        <input type="text" ref={input => this._name = input} />
        <button onClick={this.handleSubmitClick}>Sign up</button>
      </div>
    );
  }
}
```
