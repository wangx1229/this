# this
this的指向
首先要明确一点，this指向哪里，取决于函数调用的位置，而不是函数声明的位置
## 1. 普通函数调用
```javascript
var source = 'I come from window'
fn() {
  console.log(this.source)
}
fn() // I come from window
```
当我们通过fn()调用函数的时候，会发现打印出'I come from window'，说明this会指向全局对象，在浏览器中指向window，在node中指向global。

**结论：普通方式的函数调用时，会使函数中的this指向全局对象**

## 2. 通过对象的方法进行调用
```javascript
  var source = 'I come from window'
  function fn () {
    console.log(this.source)
  }
  var obj = {
    source: 'I come from obj',
    fn: fn
  }
  fn() // I come from window
  obj.fn() // I come from obj
```
观察obj中的fn属性获取fn的方式
此时如果我们调用fn(),会发现控制台打印出'I come from window',说明this指向全局对象。
而如果调用obj.fn(),会发现控制台打印出'I come from obj',说明this指向obj对象。

**结论：通过对象内部的属性访问一个方法时，方法内部的this会指向该对象。 **

## 3. 通过call，apply调用函数
```javascript
  function fn () {
    console.log(this.source)
  }
  var obj = {
    source: 'I come from obj'
  }
  fn.call(obj) // I come from obj
  fn.apply(obj) // I come from obj
```
调用call/apply方法, 会发现控制台打印出'I come from obj',说明this指向obj对象。

**结论： 通过call或者apply调用fn的时候，使得fn内部的this指向了obj对象。**

## 4. 通过new关键字改变this的指向
```javascript
  function fn (str) {
    this.source = str
  }
  var newObj = new fn('I come from params')
  console.log(newObj.source) // 'I come from params'
```
根据输出的结果，我们发现，this指向了我们新创建的newObj。

**结论：当使用new关键字调用fn函数的时候，会执行下面的操作:**

**1. 创建一个新对象，并使this指向这个对象**

**2. 新对象会被执行prototype链接**

**3. 如果这个fn函数没有返回任何对象，那么才会返回我们新创建的这个对象**

## 5. 通过箭头函数 () => {}
```javascript
  var obj = {source: 'I come from obj'}
  function fn () {
    return () => console.log(this.source)
  }
  var temp = fn.call(obj)
  temp() // I come from obj
```
fn会返回一个箭头函数，我们将obj绑定到fn中，因此在fn中this指向obj，在调用箭头函数时，箭头函数指向外层的this，所以指向obj，得到'I come from obj'

**结论： 箭头函数中的this指向外层函数作用域中的this**
