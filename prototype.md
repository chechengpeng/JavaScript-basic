## 原型与原型链

- 原型与原型链是 JS 的非常基本的概念，理解的不够透彻和清晰，特此记录分析，加深理解。

---

### 基本概念

- 首先要理清构造函数、原型和实例的关系：每个构造函数都有一个原型对象`prototype`，原型对象都包含一个指向构造函数的指针`constructor`，实例包含一个指向原型对象的内部指针`__proto__`。每个实例对象都有一个私有属性`__proto__`指向其构造函数的原型对象`prototype`，该原型对象也有自己的通过`__proto__`指向的原型对象，层层向上直到一个对象的原型对象为`null`，层层递进形成的链条，即为原型链。

### 理解原型对象

- 创建一个新函数就会为该函数创建一个`prototype`属性，这个属性指向函数的原型对象。默认情况下，所有原型对象都会自动获得一个`constructor`属性，这个属性包含一个指向`prototype`属性所在函数的指针。当用构造函数创建一个新实例后，实例内部将包含一个指针，指向构造函数的原型对象。

```js
function Person() {}
var p1 = new Person();
console.log(p1.__proto__ === Person.prototype); // 实例的__proto__指向构造函数的原型对象
console.log(Person.prototype.constructor === Person); // 构造函数的原型对象上的constructor属性指向构造函数
console.log(Object.getPrototypeOf(p1) === Person.prototype); // Object.getPrototypeOf()返回实例的原型
```

### 继承

#### 1. 原型链继承

- 本质上是重写原型对象，指向另外一个构造函数的实例
- 包含引用类型值的原型属性会被所有实例共享

#### 2. 借用构造函数（经典继承）

- 通过使用`apply()`和`call()`方法可以在新创建的对象上执行构造函数
- 方法都在构造函数中定义，函数无法复用

```js
function SuperType(name) {
  this.name = name;
}
function SubType() {
  SuperType.call(this, "AnyName");
  this.age = 30;
}
var i = new SubType();
console.log(i.name);
```

#### 3.组合继承

- 使用原型链实现对原型属性和方法的继承，使用构造函数实现对实例属性的继承

```js
function SuperType(name) {
  this.name = name;
  this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function() {
  console.log(this.name);
};
function SubType(name, age) {
  SuperType.call(this, name); // 继承属性
  this.age = age;
}

SubType.prototype = new SuperType(); // 继承方法
var i = new SubType("aname", 11);
i.sayName();
```

#### 4.原型式继承

- Object.create() 一个对象与另一个保持类似，不需要创建构造函数

#### 5.寄生式继承

- 创建一个仅用于封装继承过程的函数，并在函数内部亿某种方式增强对象
- 不能做到函数复用

```js
function createAnother(o) {
  var clone = Object.create(o);
  clone.sayHi = function() {
    console.log("hi");
  };
  return clone;
}
```

#### 6.寄生组合式继承

- 解决组合式继承会调用两次超类型构造函数
- 借用构造函数继承属性，通过原型链的变体继承方法
- 使用寄生式继承来继承超类型的原型，再将结果指定给子类型的原型

```js
function inheritPrototype(subType, superType) {
  var prototype = Object.create(superType);
  prototype.constructor = subType;
  subType.prototype = prototype;
}
```
