
# Hoisting

JavaScript moves declarations to the top of the scope during compilation.

`var` → hoisted as undefined.

`let` & `const` → hoisted but stay in Temporal Dead Zone (TDZ).

`Function` declarations are fully hoisted.

---

### Example:
```js
// 1: var hoisting
console.log(x); // undefined (hoisted)
var x = 5;

// 2: let & const hoisting
// console.log(y); // ❌ ReferenceError (TDZ)
let y = 10;

// 3: Function Declaration hoisting
sayHello(); // ✅ Works
function sayHello() {
  console.log("Hello!");
}

// 4: Function Expression hoisting
// greet(); // ❌ Error: Cannot access before initialization
const greet = function() {
  console.log("Hi!");
};
```
---

| Declaration Type     | Hoisted? | Value Before Initialization |
|-----------------------|----------|-----------------------------|
| `var`                | Yes      | `undefined` |
| `let` / `const`      | Yes (TDZ)| ReferenceError |
| Function Declaration | Yes      | Full function available |
| Function Expression  | No       | ReferenceError |