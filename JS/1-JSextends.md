---
title: JS的几种继承方式
---

### 类式继承（通过把父类的实例赋值给子类的原型来实现）

```js
function SuperClass(){

}
SuperClass.prototype.book = ['JavaScript','HTML','CSS']
function SubClass(){}
SubClass.prototype = new SuperClass()
var instance1 = new SubClass()
var instance2 = new SubClass()
console.log(instance1.book)
instance2.book.push('yewenxiang')
console.log(instance1.book)
```

缺点:
- 1.如果父类中共有属性要是有引用类型，在一个子类的实例更改了子类原型从父类构造函数，或者父类原型中继承来的共有属性，就会直接影响到其他子类
- 2.由于子类实现的继承是靠其原型prototype对父类的实例化实现的，因此在创建父类时，无法向父类传递参数，因而在实例化父类的时候也无法对父类构造函数内的属性传递参数初始化

### 构造函数继承 （通过在子类的构造函数调用父类的构造函数执行环境来实现继承）

```js
function SuperClass(id) {
  this.book = ['JavaScript','HTML','CSS']
  this.id = id
}
SuperClass.prototype.showBook = function(){
  console.log(this.book)
}
function SubClass(id){
  SuperClass.call(this,id)  //call可以更改函数的执行环境，因此在子类中，对superClass调用这个方法就是在子类中的变量在父类中执行一遍，由于父类中是给this绑定的属性，因此子类自然也继承了父类共有的属性
}

var instance1 = new SubClass(10);
var instance2 = new SubClass(11);
instance1.book.push('yewenxiang')
console.log(instance1.book)    //["JavaScript", "HTML", "CSS", "yewenxiang"]
console.log(instance1.id)      //10
console.log(instance2.book)    //["JavaScript", "HTML", "CSS"]
console.log(instance2.id)      //11
instance1.showBook()           //TypeError
```

缺点:
- 这种类型的继承没有涉及原型，所以父类的原型方法自然不会被子类继承，如果想要被子类继承就必须放在构造函数中，这样创建出来的每个实例都会单独拥有一份而不能共用，违背了代码复用的原则

### 组合继承(结合了类式继承和构造函数继承的优点)

```js
function SuperClass(name){
      this.name = name;
      this.book = ['HTML','JavaScript','CSS']
}
SuperClass.prototype.getName = function(){
  console.log(this.name)
}
function SubClass(name,time){
  SuperClass.call(this,name)
  this.time = time;
}
SubClass.prototype = new SuperClass()
SubClass.prototype.getTime = function(){
  console.log(this.time)
}
var instance1 = new SubClass('js book',2014)
instance1.book.push('yewenxiang')
console.log(instance1.book)   //["HTML", "JavaScript", "CSS", "yewenxiang"]
instance1.getName()           //js book
instance1.getTime()           //2014

var instance2 = new SubClass('css book',2015)
console.log(instance2.book)   //["HTML", "JavaScript", "CSS"]
instance2.getName()           //css book
instance2.getTime()           //2015
```

缺点:
- 在使用构造函数式继承时执行了一遍父类的构造函数，而实现子类原型的类式继承又调用了一遍父类的构造函数，因此父类构造函数调用了两遍，不是最完美的方式。

### 原型式继承（对类式继承的一个封装）

```js
function inheritObject(o){
        function F(){}
        F.prototype = o;
        return new F();
}
var book = {
  name:'js book',
  alikeBook:['css book','html book']
}
var newBook = inheritObject(book)
newBook.name = 'ajax book';
newBook.alikeBook.push('xml book')

var otherBook = inheritObject(book)
otherBook.name = 'flash book';
otherBook.alikeBook.push('as book')

console.log(book.name)             //js book
console.log(book.alikeBook)        //["css book", "html book", "xml book", "as book"]
console.log(newBook.name)          //ajax book
console.log(newBook.alikeBook)     //["css book", "html book", "xml book", "as book"]
console.log(otherBook.name)        //flash book
console.log(otherBook.alikeBook)   //["css book", "html book", "xml book", "as book"]
```

优点:
- 和类式继承类似，不过由于F过渡类的构造函数无内容，开销比较小

缺点:
- 类式继承的缺点

### 寄生式继承 （对原型继承的二次封装）

```js
var book = {
        name:'js book',
        alikeBook:['css book','html book']
      }
function createBook(obj){
  var o = new inheritObject(obj)
  o.getName = function(){
    console.log(name)
  }
  return o
}
```

解释:
- 对原型继承的二次封装,并在二次封装的过程中对继承的对象进行扩展，新创建的对象不仅有父类的属性和方法，而且添加了新的属性和方法

### 寄生组合式继承

```js
function inheritObject(o){
      function F(){}
      F.prototype = o;
      return new F();
}
function inheritPrototype(subClass, superClass){
  var p = inheritObject(superClass.prototype)
  p.constructor = subClass
  subClass.prototype = p
}

function SuperClass(name){
  this.name = name;
  this.color=['red','blue','green'];
}
SuperClass.prototype.getName = function(){
  console.log(this.name)
}

function SubClass(name, time){
  SuperClass.call(this,name)
  this.time = time;
}
inheritPrototype(SubClass,SuperClass)
SubClass.prototype.getTime  = function(){
  console.log(this.time)
}
var instance1 = new SubClass('js book', 2014)
var instance2 = new SubClass('js book', 2015)

instance1.color.push('black')
console.log(instance1.color)   //["red", "blue", "green", "black"]
console.log(instance2.color)   //["red", "blue", "green"]
instance1.getTime()            //2014
instance2.getName()            //js book
```

>需要注意的是:子类想添加原型和方法必须通过 .prototype 来添加，直接赋予对象会覆盖掉从父类原型继承的对象了
>
