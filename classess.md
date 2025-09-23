# JavaScript Classes

In JavaScript, **classes** and **objects** are the foundation of Object-Oriented Programming (OOP).
- A **class** is a blueprint (template) to create objects with similar properties and behaviors.
- An **object** is an instance of a class.

👉 Example:
- The animal type **Dog** is a class.
- A specific dog named **Tommy** is an object of the `Dog` class.

---

## 1. Constructor Method
The `constructor` method is a special function with the name `constructor`.
It is automatically executed when a new object is created, mainly used to **initialize object properties**.

```js
class Dog {
  static sound = "bark"; // class property

  constructor(name) {
    this.name = name; // instance property
    console.log('Dog Name is',this.name)
  }
}
const myDog = new Dog("Rayne");
```

---

## 2. Class Methods
You can define methods directly in a class. These methods are available to instances of the class.

```js
class Dog {
  constructor(name, breed) {
    this.name = name;
    this.breed = breed;
  }

  bark() {
    console.log(`The dog Name is ${this.name}, & breed is ${this.breed}, says woof!`);
  }
}

const myDog = new Dog("Rayne", "Husky");
myDog.bark(); // Rayne, the Husky, says woof!
```
---

## 3. Getters and Setters
Use `get` to define a **getter** and `set` to define a **setter** for controlled property access.

```js
class Dog {
  constructor(name) {
    this.name = name;
  }

  get dogName() {
    return this.name;
  }

  set dogName(newName) {
    this.name = newName;
  }

  bark() {
    console.log(`${this.name} says woof!`);
  }
}

let myDog = new Dog("Rayne");
console.log(myDog.dogName); // Rayne

myDog.dogName = "Buddy";
console.log(myDog.dogName); // Buddy
```

---

## 4. Public Class Fields
Fields can be declared directly inside the class body.

```js
class Car {
  wheels = 4; // public field

  constructor(model) {
    this.model = model;
  }
}

const c = new Car("Tesla");
console.log(c.model);  // Tesla
console.log(c.wheels); // 4
```

---

## 5. Private Fields and Methods
Fields/methods prefixed with `#` are **private** and inaccessible outside the class.

```js
class BankAccount {
  #balance = 0;

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const acc = new BankAccount();
acc.deposit(200);
console.log(acc.getBalance()); // 200
// acc.#balance ❌ Error
```

---

## 6. Static Properties and Methods
- **Instance methods/properties** → belong to objects created from the class.
- **Static methods/properties** → belong to the class itself (not instances).

```js
class MathUtil {
  static PI = 3.14159;

  static square(n) {
    return n * n;
  }
}

console.log(MathUtil.PI);        // 3.14159
console.log(MathUtil.square(5)); // 25

const obj = new MathUtil();
console.log(obj.PI); // ❌ undefined
```

---

## 7. Inheritance (`extends` and `super`)
Classes can inherit from others using `extends`.
Use `super` to call the parent constructor or methods.

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(`${this.name} makes a sound`);
  }
}

class Dog extends Animal {
  speak() {
    super.speak();
    console.log(`${this.name} barks`);
  }
}

const d = new Dog("Tommy");
d.speak();
// Tommy makes a sound
// Tommy barks
```

---

## 8. Advanced Class Concepts
### a. Class Expressions
Classes can be named within expressions.

a. Unnamed Class Expression
```js
const Animal = class {
  constructor(name) {
    this.name = name;
  }
};
const a = new Animal("Leo");
console.log(a.name); // Leo
```

b. Named Class Expression
```js
const Cat = class Feline {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} meows`);
  }
};

const kitty = new Cat("Milo");
kitty.speak(); // Milo meows
```
	•	Use unnamed if you don’t need the class’s internal name.
	•	Use named when you might need self-reference (e.g., recursive static methods, debugging stack traces).

```js
const Factorial = class Fact {
  static calculate(n) {
    if (n <= 1) return 1;
    return n * Fact.calculate(n - 1); // recursive call using "Fact"
  }
};
console.log(Factorial.calculate(5)); // 120
```
```js
const Cat = class Feline {
  constructor(name) {
    this.name = name;
    }
  meow() {
    throw new Error("Oops!");
    }
};
new Cat("Milo").meow();
```
Stack trace will include Feline.meow, not just AnonymousClass.

---

### b. Abstract-like Classes (Simulation)
JavaScript doesn’t have true abstract classes, but you can simulate them.
	•	An abstract class is a blueprint: you cannot build it directly, only its children can.
	•	Children must implement certain methods (like speak() here).

```js
// Abstract-like class simulation
class Animal {
  constructor() {
    if (this.constructor === Animal) {
      throw new Error("Abstract class can't be instantiated");
    }
  }

  speak() {
    throw new Error("Method must be implemented");
  }
}

// ✅ Valid: Subclass implementing the abstract method
class Dog extends Animal {
  speak() {
    console.log("Woof");
  }
}

const dog = new Dog();
dog.speak(); // Woof


// ❌ Invalid Case 1: Direct instantiation of abstract class
try {
  const a = new Animal();
} catch (e) {
  console.error(e.message); // Abstract class can't be instantiated
}


// ❌ Invalid Case 2: Subclass without overriding abstract method
class Cat extends Animal {
    speak() {
    throw new Error("Method must be implemented");
  }
}

try {
  const kitty = new Cat();
  kitty.speak(); // Will throw an error
} catch (e) {
  console.error(e.message); // Method must be implemented
}
```
---

### c. Mixins (Multiple Inheritance Simulation)
A mixin is a function that:
	•	Takes a base class as input.
	•	Returns a new class that extends the base with extra methods/properties.
Think of it as a “behavior pack” that can be added to a class.

```js
let CanFly = Base => class extends Base {
  fly() {
    console.log("Flying");
    }
};

let CanSwim = Base => class extends Base {
  swim() {
    console.log("Swimming");
    }
};

class Animal {}
class Duck extends CanFly(CanSwim(Animal)) {}

const d = new Duck();
d.fly();  // Flying
d.swim(); // Swimming
```

---
### d. Bound Methods with Class Fields
Problem: Losing `this` in callbacks.

```js
class Button {
  constructor(value) {
    this.value = value;
  }
  click() {
    alert(this.value);
  }
}

let button = new Button("hello");
setTimeout(button.click, 1000); // undefined ❌
```

✅ Fix using class fields:

```js
class Button {
  constructor(value) {
    this.value = value;
  }
  click = () => {
    alert(this.value);
  };
}

let button = new Button("hello");
setTimeout(button.click, 1000); // hello
```
---

## 9. Pros and Cons of Classes
**Pros**
- Cleaner syntax than prototypes.
- Easier for OOP developers to understand.
- Supports inheritance, static members, getters/setters.
- Encapsulation using `#private` fields.

**Cons**
- No true private methods until `#` syntax.
- No built-in multiple inheritance.
- Can lead to memory overhead in large apps.
---

## 10. Interview-Oriented Questions
### Q1. Class vs Constructor Function
```js
function AnimalOld(name) {
  this.name = name;
  }
AnimalOld.prototype.speak = function() {
  console.log(this.name);
}

class AnimalNew {
  constructor(name) {
    this.name = name;
    }
  speak() {
    console.log(this.name);
    }
}
```
---

### Q2. Private vs Public Fields
```js
class Person {
  #ssn;         // if add # then private variable
  constructor(name, ssn) {
    this.name = name;
    this.#ssn = ssn;
  }
}
```
---

### Q3. Static vs Instance Methods
```js
class A {
  greet() {
    console.log("Hi");
    }      // instance
  static greetStatic() {
    console.log("Hi");
    } // static
}

new A().greet(); // Hi
A.greetStatic(); // Hi
```
---

### Q5. Inheriting Static Methods
```js
class Parent {
  static hello() {
    return "Hello";
    } }
class Child extends Parent {}
console.log(Child.hello()); // Hello
```
---
### Q6. `super` Keyword
```js
class Person {
  constructor(name) {
    this.name = name;
    }
  sayHello() {
    console.log(`Hi, I'm ${this.name}`);
    }
}

class Employee extends Person {
  constructor(name, company) {
    super(name);
    this.company = company;
  }
  sayHello() {
    super.sayHello();
    console.log(`I work at ${this.company}`);
  }
}
new Employee("Sam", "Google").sayHello();
// Hi, I'm Sam
// I work at Google
```
---

### Q8. Polymorphism Example
```js
class Employee {
  calculateSalary() {
    return 0;
    }
}
class Manager extends Employee {
  constructor(base) {
    super();
    this.base = base;
    }
  calculateSalary() {
    return this.base + 5000;
    }
}
class Developer extends Employee {
  constructor(base) {
    super();
    this.base = base;
    }
  calculateSalary() {
    return this.base + 2000;
    }
}

const m = new Manager(30000);
const d = new Developer(25000);
console.log(m.calculateSalary()); // 35000
console.log(d.calculateSalary()); // 27000
```