
# JavaScript Prototypes - From Basics to Advanced

## Introduction
In JavaScript, **prototypes** are the backbone of inheritance and code reuse.
Understanding prototypes helps in building memory-efficient and flexible applications.
While ES6 `class` syntax is more common today, under the hood, JavaScript uses prototypes.

---
## 1. What is a Prototype?
In JavaScript, every object has a hidden property called [[Prototype]] (accessible via __proto__ in most environments).
It’s basically a link to another object from which it can inherit properties and methods.

Example:
```js
Object.prototype.print = function () {
    console.log('I am from object prototype');
}

let b = {
  name: 'Pranjal',
  age: 21
};
b.print(); // I am from object prototype
```

---
## 2. How Prototype Works
- Every object has a `[[Prototype]]` pointing to another object.
- JS looks for a property/method first on the object, then up the prototype chain.

Example:
```js
const person = { name: "Alice" };
console.log(person.toString);
// Not in person, found in Object.prototype
```

---
## 3. Constructor Functions
Used to create objects and initialize properties.
```js
function Person(name) {
    this.name = name;
}
Person.prototype.sayHello = function () {
    console.log(`Hello, my name is ${this.name}`)
}
const p = new Person("Sheema");
p.sayHello(); // Hello, my name is Sheema
```

---
## 4. Adding Methods to Prototype
Methods added to a prototype are shared across all instances.

```js
Array.prototype.sum = function () {
    return this.reduce((acc, val) => acc + val, 0);
};

let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];

console.log(arr1.sum()); // 6
console.log(arr2.sum()); // 15
```

---
## 5. Prototype Chain
Objects inherit features through the prototype chain.

```js
function Animal(type) {
  this.type = type;
}
Animal.prototype.speak = function () {
  console.log(`${this.type} makes a sound`);
}
function Dog(name) {
  this.name = name;
}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
Dog.prototype.bark = function () {
   console.log(`${this.name} barks`);
}

const d = new Dog("Tommy");
d.speak(); // inherited from Animal
d.bark();  // own method
```

*Note*: `d.type` is undefined because Dog constructor does not call Animal constructor.

---
## 6. Advanced Prototype Concepts

### a) Object.create()
Creates a new object with a specified prototype.

```js
const animal = { eats: true };
const rabbit = Object.create(animal);
console.log(rabbit.eats); // true
```

### b) Dynamic Prototype Modification
```js
function Person(name) {
  this.name = name;
}
const p1 = new Person("Alice");
Person.prototype.greet = function() {
  console.log(`Hi, ${this.name}`);
}
p1.greet(); // Hi, Alice
```

### c) Extending Built-in Objects
```js
String.prototype.reverse = function() {
    return this.split('').reverse().join('');
}
console.log("hello".reverse()); // olleh
```

### d) Prototype vs `__proto__`
```js
function Car(model) {
  this.model = model;
}
Car.prototype.drive = function () {
  console.log(`${this.model} is driving...`);
};

const c1 = new Car("Tesla");
console.log(c1.__proto__ === Car.prototype); // true
```

### e) Property Shadowing in Prototype Chain
```js
let obj = { name: "Local" };
Object.prototype.name = "From Prototype";

console.log(obj.name); // "Local"
delete obj.name;
console.log(obj.name); // "From Prototype"
```

---
## 7. Interview Questions on Prototypes

1. **What is a prototype in JS?**
   - An object from which other objects inherit properties and methods.

2. **Difference between `__proto__` and `prototype`?**
   - `prototype` is on constructor functions, `__proto__` is on objects.

3. **How does prototype chain work?**
   - JS searches for properties up the chain until it finds the property or reaches `null`.

4. **What happens if you modify `Object.prototype`?**
   - All objects inherit the modification. Generally unsafe in production.

5. **How to create inheritance without classes?**
   - Using `Object.create()` or constructor function + prototype.

6. **Dynamic prototype: what is it?**
   - Adding or changing prototype methods at runtime.

7. **Why use prototypes over instance methods?**
   - Memory efficiency: methods are shared among all instances.

---
## Conclusion
- Prototypes form the foundation of JavaScript inheritance and memory optimization.
- Classes use prototypes internally, so mastering prototypes helps write efficient and maintainable code.
- Advanced usage includes `Object.create`, dynamic methods, and extending built-ins.
