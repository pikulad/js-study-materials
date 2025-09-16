---
marp: true
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('Assets/background.svg')
---

![bg left:40% 80%](Assets/codeastu_ciklum.png)

# **Javascript Essentials**

`this` Keyword

---

# **`this` Keyword**

- The `this` keyword in JavaScript refers to the object to which it belongs
- Its value is determined by how a function is called, making it a dynamic reference
- It's a powerful concept used to access properties and methods of an object
- Allows for more flexible and reusable code
----
```js
const person = {
    name: "Codeastu",
    greet() {
        return `Welcome To, ${this.name}`;
    }
};
console.log(person.greet()); // Welcome To, Codeastu
```

### 1. `this` in Object Methods

- In the context of an object method, `this` refers to the object itself
- Allows access to the object's properties and methods within the method's scope
- Facilitates interaction with the object's data and behavior
---
```js
const car = {
    brand: "Toyota",
    model: "Camry",
    year: 2023,
    getInfo() {
        return `${this.year} ${this.brand} ${this.model}`;
    },
    start() {
        console.log(`Starting the ${this.brand} ${this.model}`);
    }
};

console.log(car.getInfo()); // 2023 Toyota Camry
car.start(); // Starting the Toyota Camry
```
---
**References**
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this

---

### 2. `this` in Global Context

- In the global context (outside any function), `this` refers to the global object
- In browsers, the global object is `window`
- In Node.js, the global object is `global`
---
```js
console.log(this === window); // true (in browser)

var globalVar = "I'm global";
console.log(this.globalVar); // "I'm global"

function globalFunction() {
    console.log(this === window); // true
}
globalFunction();
```

**Notes**
In strict mode, `this` in global context is `undefined`

---

### 3. `this` in Function Context

- In a regular function, `this` depends on how the function is called
- If called directly, `this` refers to the global object (or `undefined` in strict mode)
- If called as a method, `this` refers to the object that owns the method
---
```js
function regularFunction() {
    console.log(this); // window (or undefined in strict mode)
}

const obj = {
    method: function() {
        console.log(this); // obj
    }
};

regularFunction(); // window
obj.method(); // obj
```

---

### 4. `this` in Arrow Functions

- Arrow functions don't have their own `this` binding
- They inherit `this` from the enclosing lexical scope
- Arrow functions cannot be used as constructors
---
```js
const obj = {
    name: "Arrow Function Example",
    regularMethod: function() {
        console.log("Regular:", this.name); // Arrow Function Example
        
        const arrowFunction = () => {
            console.log("Arrow:", this.name); // Arrow Function Example
        };
        arrowFunction();
    }
};

obj.regularMethod();
```

**Notes**
Arrow functions are great for callbacks where you want to preserve the original `this` context

---

### 5. `this` in Event Handlers

- In DOM event handlers, `this` refers to the element that received the event
- This allows you to access the element's properties and methods
---
```js
// HTML: <button id="myButton">Click me</button>

document.getElementById('myButton').addEventListener('click', function() {
    console.log(this); // <button id="myButton">Click me</button>
    console.log(this.id); // "myButton"
    this.style.backgroundColor = 'red';
});

// With arrow function (this refers to window)
document.getElementById('myButton').addEventListener('click', () => {
    console.log(this); // window object
});
```

---

### 6. `this` in Constructor Functions

- When a function is used as a constructor with the `new` keyword, `this` refers to the newly created object
- The constructor function is used to initialize the object's properties

**References**
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new

---
```js
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.greet = function() {
        return `Hello, I'm ${this.name} and I'm ${this.age} years old`;
    };
}

const person1 = new Person("Alice", 30);
const person2 = new Person("Bob", 25);

console.log(person1.greet()); // Hello, I'm Alice and I'm 30 years old
console.log(person2.greet()); // Hello, I'm Bob and I'm 25 years old
```

---

### 7. `this` with `call()` Method

- The `call()` method allows you to call a function with a specific `this` value
- You can pass arguments individually after the `this` value
---
```js
const person1 = {
    name: "Alice",
    greet: function(greeting, punctuation) {
        return `${greeting}, I'm ${this.name}${punctuation}`;
    }
};

const person2 = {
    name: "Bob"
};
// Using call to set this to person2
const result = person1.greet.call(person2, "Hello", "!");
console.log(result); // Hello, I'm Bob!
```

**Notes**
`call()` immediately invokes the function with the specified `this` context

---

### 8. `this` with `apply()` Method

- The `apply()` method is similar to `call()` but accepts arguments as an array
- Useful when you have an array of arguments to pass
---
```js
const calculator = {
    operation: "addition",
    calculate: function(a, b, c) {
        return `${this.operation}: ${a + b + c}`;
    }
};

const math = {
    operation: "multiplication"
};

// Using apply with array of arguments
const result = calculator.calculate.apply(math, [2, 3, 4]);
console.log(result); // multiplication: 9

// Using call with individual arguments
const result2 = calculator.calculate.call(math, 2, 3, 4);
console.log(result2); // multiplication: 9
```

---

### 9. `this` with `bind()` Method

- The `bind()` method creates a new function with a specific `this` value
- Unlike `call()` and `apply()`, `bind()` doesn't immediately invoke the function
- Returns a new function that can be called later
---
```js
const person = {
    name: "Charlie",
    greet: function() {
        return `Hello, I'm ${this.name}`;
    }
};

const boundGreet = person.greet.bind(person);
console.log(boundGreet()); // Hello, I'm Charlie

// Using bind with different context
const anotherPerson = { name: "Diana" };
const boundGreet2 = person.greet.bind(anotherPerson);
console.log(boundGreet2()); // Hello, I'm Diana
```
---
**Notes**
`bind()` is commonly used in event handlers and callbacks to preserve `this` context

---

### 10. `this` in Class Methods

- In ES6 classes, `this` refers to the instance of the class
- Methods defined in a class automatically bind `this` to the instance
- Arrow functions in classes bind `this` to the class instance


**References**
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes

---
```js
class Car {
    constructor(brand, model) {
        this.brand = brand;
        this.model = model;
    }
    
    // Regular method - this refers to the instance
    getInfo() {
        return `${this.brand} ${this.model}`;
    }
    
    // Arrow function method - this also refers to the instance
    start = () => {
        console.log(`Starting ${this.brand} ${this.model}`);
    }
}

const myCar = new Car("Honda", "Civic");
console.log(myCar.getInfo()); // Honda Civic
myCar.start(); // Starting Honda Civic
```
---

### 11. Common `this` Pitfalls and Solutions

- Losing `this` context when passing methods as callbacks
- Solutions: Use `bind()`, arrow functions, or store `this` in a variable

---
```js
const obj = {
    name: "Context Example",
    data: [1, 2, 3, 4, 5],
    
    // Problem: this is lost in forEach callback
    problemMethod: function() {
        this.data.forEach(function(item) {
            console.log(this.name, item); // undefined 1, undefined 2, etc.
        });
    },
    
    // Solution 1: Use bind()
    solution1: function() {
        this.data.forEach(function(item) {
            console.log(this.name, item);
        }.bind(this));
    },
    
    // Solution 2: Use arrow function
    solution2: function() {
        this.data.forEach(item => {
            console.log(this.name, item);
        });
    },
    
    // Solution 3: Store this in variable
    solution3: function() {
        const self = this;
        this.data.forEach(function(item) {
            console.log(self.name, item);
        });
    }
};
```

---

### 12. `this` in Strict Mode

- In strict mode, `this` behaves differently
- Global `this` is `undefined` instead of the global object
- Helps catch common mistakes and makes code more predictable
---
```js
"use strict";

function strictFunction() {
    console.log(this); // undefined (not window)
}

function nonStrictFunction() {
    console.log(this); // window object
}

strictFunction();
nonStrictFunction();

// In strict mode, this in methods still works normally
const obj = {
    method: function() {
        "use strict";
        console.log(this); // obj
    }
};
obj.method();
```
---
**Notes**
Always use strict mode for better error catching and cleaner code

---

### 13. `this` Best Practices

- **Be explicit**: Use `bind()`, `call()`, or `apply()` when you need specific `this` context
- **Use arrow functions**: For callbacks where you want to preserve the outer `this`
- **Avoid losing context**: Be careful when passing methods as callbacks
- **Use strict mode**: Helps catch `this`-related errors
- **Understand the context**: Always know what `this` refers to in your current scope
---
```js
// Good: Explicit binding
const boundMethod = obj.method.bind(obj);
setTimeout(boundMethod, 1000);

// Good: Arrow function preserves this
class Component {
    constructor() {
        this.name = "Component";
    }
    
    render() {
        // Arrow function preserves this
        setTimeout(() => {
            console.log(this.name); // Component
        }, 1000);
    }
}
```
---
**References**
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind
