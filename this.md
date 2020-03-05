## this 学习笔记

### 为什么要用 this

- `this`提供了一种更优雅的方式来隐式传递一个对象的引用，可以将 API 设计的简洁并易于复用。

```js
function identify() {
  return this.name.toUpperCase();
}
function speak() {
  var greeting = "Hello, I am " + identify.call(this);
  console.log(greeting);
}
var me = {
  name: "Kyle"
};
var you = {
  name: "Reader"
};
identify.call(me);
identify.call(you);
speak.call(me);
speak.call(you);
```

- 如果不使用`this`，那就需要给函数显式传入一个上下文对象

```js
function identify(context) {
  return context.name.toUpperCase();
}
function speak(context) {
  var greeting = "Hello, I am " + identify(context);
  console.log(greeting);
}
speak(me);
```

### What is this

- `this`既不指向函数自身也不指向函数的词法作用域，在函数调用时发生绑定，当一个函数被调用时，会创建一个活动记录（执行上下文），其包含函数在哪里被调用，函数的调用方式，传入的参数信息。`this`就是它的一个属性，会在函数执行的过程中用到。

### 绑定规则

#### 1.默认绑定

- 使用不带任何修饰的函数进行调用，应用`this`的默认绑定，指向全局对象，严格模式下绑定到`undefined`

```js
function foo() {
  console.log(this.a);
}
var a = 2;
foo(); // 2
```

#### 2.隐式绑定

```js
function foo() {
  console.log(this.a);
}
var obj = {
  a: 2,
  foo: foo
};
obj.foo(); // 2
```

- 对象属性引用链中只有上一层或者说最后一层在调用位置中起作用

```js
function foo() {
  console.log(this.a);
}
var obj2 = {
  a: 42,
  foo: foo
};
var obj1 = {
  a: 2,
  obj2: obj2
};
obj1.obj2.foo(); // 42
```

#### 3.显式绑定

- 使用`call()` 和 `apply()` 方法

```js
function foo() {
  console.log(this.a);
}
var obj = {
  a: 2
};
foo.call(obj); // 2
```

#### 4.new 绑定

- 使用`new`来调用 foo 时，我们会构造一个新对象并把其绑定到 foo 调用的 this 上

```js
function foo(a) {
  this.a = a;
}
var bar = new foo(2);
console.log(bar.a); // 2
```
