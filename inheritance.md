# Inheritance in JavaScript

## Introduction
JavaScript is a **prototype-based language**, which means inheritance is implemented via the prototype chain rather than classical classes.
With **ES6**, JavaScript introduced the `class` syntax — syntactic sugar over prototypes — making inheritance more familiar to developers from classical OOP backgrounds.

Inheritance allows code reuse, reduces duplication, and makes systems more maintainable.

---

## 1. Prototype Inheritance (Pre-ES6)
```javascript
function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function () {
    console.log(`${this.name} makes a noise.`);
};

function Dog(name) {
    Animal.call(this, name); // Call parent constructor
}

Dog.prototype = Object.create(Animal.prototype); // Inherit prototype
Dog.prototype.constructor = Dog;

Dog.prototype.speak = function () {
    console.log(`${this.name} barks.`);
};

const dog = new Dog('Rex');
dog.speak(); // Rex barks.
```

## 2. ES6 Class Inheritance
The `class` and `extends` keywords simplify inheritance.

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    speak() {
        console.log(`${this.name} makes a noise.`);
    }
}
class Dog extends Animal {
    speak() {
        super.speak(); // Call parent method
        console.log(`${this.name} barks.`);
    }
}
const d = new Dog('Tommy');
d.speak();
// Tommy makes a noise.
// Tommy barks.
```

## 3. Multiple Inheritance with Mixins
JavaScript does not support multiple inheritance directly, but **mixins** can simulate it.

```javascript
let CanEat = {
    eat() {
        console.log(`${this.name} is eating`);
    }
};
let CanSwim = {
    swim() {
        console.log(`${this.name} is swimming`);
    }
};
class Fish {
    constructor(name) {
        this.name = name;
    }
}
// Apply mixins
Object.assign(Fish.prototype, CanEat, CanSwim);
const goldfish = new Fish("Nemo");
goldfish.eat();  // Nemo is eating
goldfish.swim(); // Nemo is swimming
```

## 5. Practical Use Cases of Inheritance
### a) Extending Built-in Classes
```javascript
class CustomArray extends Array {
    first() {
        return this[0];
    }
}
const arr = new CustomArray(10, 20, 30);
console.log(arr.first()); // 10
```

### b) Reusable Base Classes in Apps
```javascript
class ApiService {
    fetch(url) {
        return fetch(url).then(res => res.json());
    }
}
class UserService extends ApiService {
    getUsers() {
        return this.fetch("/api/users");
    }
}
```

## 6. Advanced Patterns
### a) Dynamic Inheritance (factory)
```javascript
function withLogger(Base) {
    return class extends Base {
        log(msg) {
            console.log(`[${this.constructor.name}]: ${msg}`);
        }
    };
}

class BaseModel {}
class User extends withLogger(BaseModel) {}
const u = new User();
u.log("User created"); // [User]: User created
```

### b) Composition vs. Inheritance
Sometimes **composition** is better than inheritance (more flexible, avoids deep hierarchies).

```javascript
const canFly = obj => ({
    fly: () => console.log(`${obj.name} is flying!`)
});

const canSing = obj => ({
    sing: () => console.log(`${obj.name} is singing!`)
});

function Bird(name) {
    let bird = { name };
    return Object.assign(bird, canFly(bird), canSing(bird));
}

const parrot = Bird("Polly");
parrot.fly();
parrot.sing();
```

## 7. Pros & Cons of Inheritance

### ✅ Pros
- Code reuse (avoid duplication).
- Organizes related classes hierarchically.
- Promotes DRY principles.

### ❌ Cons
- Deep inheritance chains = **hard to debug & maintain**.
- Changes in parent class may break child classes (**fragile base class problem**).
- **Multiple inheritance** not supported → need mixins/composition.
- Composition is often more flexible in large systems.

## 8. Common Interview Questions

1. **How does prototype inheritance work in JS?**
   → Objects reference methods from their prototype chain instead of copying them.

2. **Difference between `Object.create()` and `class extends`?**
   → `Object.create()` is prototype-based, while `class` uses syntactic sugar with `extends`.

3. **What are the drawbacks of inheritance?**
   → Fragile base class problem, tight coupling, difficulty in refactoring.

4. **When to prefer composition over inheritance?**
   → When behaviors need to be reused across unrelated classes.

## Conclusion
- Prototypes form the foundation of JavaScript inheritance.
- ES6 `class` syntax makes it easier and more readable.
- Advanced patterns like mixins, dynamic inheritance, and composition provide flexibility.
- Deep understanding of inheritance vs. composition is crucial for **scalable and maintainable applications**.
