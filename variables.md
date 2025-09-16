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

Variables

---
# Variables

In JavaScript, variables are used to store and manipulate data. They are defined using the `var`, `let`, or `const` keywords.

## Declaring Variables

To declare a variable, you can use the `var`, `let`, or `const` keyword followed by the variable name. For example:

```js
var age = 25;
let name = 'John Doe';
const pi = 3.14;
```
---
### Variable Scope
The scope of a variable determines where it is accessible in your code. Variables declared with var have function scope or global scope, while variables declared with let or const have block scope.

---

### Variable Types
JavaScript is a dynamically typed language, which means you don't need to explicitly declare the type of a variable. However, there are several built-in types that variables can have:

`number`: Represents numeric values.
`string`: Represents text.
`boolean`: Represents true or false values.
`null`: Represents the absence of any object value.
`undefined`: Represents an uninitialized variable or a property that does not exist.

---

### Modifying Variables
You can modify the value of a variable by assigning a new value to it. For example:

```js
var age = 25;
age = 30; // Modifying the value of the variable
```
---

### Constant Variables
Constant variables are declared using the const keyword and cannot be reassigned once they are defined. For example:

```js
const PI = 3.14;
PI = 3.14159; // This will result in an error
```
---

### Examples:
```js
// var
var a = 10;
var a = 20; // re-declare allowed
console.log(a); // 20

// let
let b = 30;
// let b = 40; // ❌ Error: Cannot re-declare
b = 40; // ✅ can update
console.log(b); // 40

// const
const c = 50;
// c = 60; // ❌ Error: Cannot re-assign
console.log(c); // 50
```
---

| Keyword | Scope         | Hoisting Behavior       | Re-declare | Re-assign |
|---------|--------------|-------------------------|------------|-----------|
| `var`   | Function      | Hoisted as `undefined` | ✅ Allowed | ✅ Allowed |
| `let`   | Block         | Hoisted (TDZ*)         | ❌ Not allowed | ✅ Allowed |
| `const` | Block         | Hoisted (TDZ*)         | ❌ Not allowed | ❌ Not allowed |

*TDZ = Temporal Dead Zone (access before declaration → ReferenceError)*