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

ES6 — Arrow Functions (`=>`)

---

## Summary
Arrow functions provide a concise syntax and lexical `this`—they capture `this` from the enclosing scope. They do not have their own `this`, `arguments`, `super`, or `new.target`, and they cannot be used as constructors.

##### why `Arrow Functions` was introduced.
- Shorter syntax – less boilerplate for writing functions.
- Lexical this – inherits this from the surrounding scope.
- Implicit return – single-expression functions return automatically.
- Better for callbacks – ideal for array methods like map, filter, reduce.
- No own arguments object – simplifies handling of function parameters with rest syntax.
- Cleaner functional programming – makes code more readable and expressive.
---

## 1. Syntax & forms
- **Concise body**: `(a, b) => a + b` (implicit return)  
- **Block body**: `(a, b) => { return a + b }` (explicit return)  
- **Single param**: `x => x * 2` (parens optional)  
- **No param**: `() => 42`

```
const add = (a, b) => a + b;
const square = n => n * n;
const noop = () => {};
```

---

## 2. Lexical `this` & use cases
- Arrow functions **capture** `this` from the lexical (outer) scope. They are ideal for callbacks where you intend to use the surrounding `this` without needing `.bind(this)`.

```
function Timer() {
  this.seconds = 0;
  setInterval(() => {
    this.seconds++
    console.log("seconds::", this.seconds);
  }, 1000); // `this` refers to Timer instance
}
```

- Note: class instance fields using arrow functions create per-instance bound methods:
```
class C {
  value = 0;
  inc = () => { this.value++; } // function created per instance, auto-bound
}
```

---

## 3. No `arguments` & no `new`
- Arrow functions have no `arguments` object. Use rest `(...args)` or access `arguments` lexically from an outer non-arrow function.
- Arrow functions cannot be constructor functions; `new arrowFn()` throws.

```
const fn = (...args) => args.length;
fn(1,2); // 2
```

---

## 4. Subtle pitfalls
1. **Methods on objects**: Avoid arrow functions for object methods that use `this` to refer to the object itself:  
```
const obj = {
  count: 0,
  inc: () => { this.count++; } // `this` not obj
};
```
2. **Prototype methods**: Arrow functions don't have `prototype`. Do not use for methods intended to be on prototypes or constructors.  
3. **Per-instance allocation**: Arrow methods in class fields create a new function per instance (useful for binding but memory cost for many instances).

---

## 5. Advanced patterns & examples
### Currying & partial application
```
const curry = f => a => b => f(a, b);
const add = (a, b) => a + b;
const add1 = curry(add)(1);
console.log(add1(2)); // 3
```

### Composition & point-free style
```
const compose = (f, g) => x => f(g(x));
const toUpper = s => s.toUpperCase();
const exclaim = s => s + '!';
const shout = compose(exclaim, toUpper);
console.log(shout('hello')); // HELLO!
```

### Implicit object return requires parentheses
```
const make = (id) => ({ id }); // parentheses avoid ambiguity with block body
console.log(make(2)); // { id: 2 }
```

---

## 6. Performance & style guidance
- Use arrow functions for small callbacks and one-off functions.  
- For library/public API methods prefer named functions (better stack traces).  
- Beware of creating many per-instance arrow functions if you have thousands of objects — preferable to place methods on prototype unless you need lexical binding.

---

## 7. Interview Questions (easy → hard)
##### 1. What is lexical `this` and how do arrow functions implement it?
- **Lexical `this`** means `this` is **determined by the surrounding scope**, not by how the function is called.
- Arrow functions **do not have their own `this`**. They inherit `this` from the parent context.
- Useful in callbacks and nested functions where you want `this` to refer to the outer object.

##### 2. Why are arrow functions not constructible?
- Arrow functions **cannot be used with `new`**.
- They **do not have a `[[Construct]]` method**, which is required for constructor calls.
- Attempting `new` on an arrow function throws a **TypeError**.
- They are intended only as lightweight, non-constructible function expressions.

##### 3. How would you access `arguments` inside an arrow function?
- Arrow functions **do not have their own `arguments` object**.
- To access function arguments:
  - Use **rest parameters (`...args`)** instead.
- Example:
```
const sum = (...args) => args.reduce((a, b) => a + b, 0);
```

##### 4. Explain the memory tradeoffs of using arrow functions as class fields
- Arrow functions defined in **class fields** are created **per instance**, not on the prototype.
- **Memory impact**:
  - Each instance holds its own function, increasing memory usage if many instances exist.
  - Prototype methods are shared across all instances, saving memory.
- **Tradeoff**: convenience of lexical `this` vs. higher memory usage.

##### 5. Demonstrate a bug caused by an arrow used as an object method and fix it
- **Bug example**:
```
const obj = {
  value: 10,
  getValue: () => this.value
};
console.log(obj.getValue()); // undefined
```
  - `this` is lexical and refers to the outer scope, not `obj`.

- **Fix**: Use a regular function to bind `this` to the object:
```
const obj = {
  value: 10,
  getValue() { return this.value; }
};
console.log(obj.getValue()); // 10
```
---

## References
- MDN: [Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- ECMAScript spec: [Arrow Functions](https://tc39.es/ecma262/#prod-ArrowFunction)
