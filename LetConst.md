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

ES6 — `let` & `const`

---

## Summary
`let` and `const` introduce block-scoped bindings to JavaScript. `let` creates a mutable binding; `const` creates an immutable binding (the reference is immutable, the value may still be mutable). They replace many of the problems caused by `var` and enable safer, clearer code.

##### Problems with var (before ES6)

- Function scope only → var ignores block scope (if, for, etc.), leading to bugs.
- Hoisting confusion → variables declared with var are hoisted and initialized as undefined.
- Global pollution → accidentally redeclaring variables overwrote existing ones.
- Loop closure issues → var doesn’t create new bindings per loop iteration, causing unexpected behavior in callbacks.

##### Why let was introduced

- Provides block scope → variables exist only within { }.
- Eliminates accidental overwrites by restricting scope.
-Still allows reassignment, unlike const.
- Solves loop closure problem by creating new binding in each iteration.
- Safer than var, reducing scope-related bugs.

##### Why const was introduced

- Declares constants (cannot be reassigned).
- Makes code more predictable by enforcing immutability of bindings.
- Encourages good coding practices → using constants when values should not change.
- Still block-scoped (like let).
- Improves readability and maintainability → makes intent clear (value should not be reassigned).
---

## 1. Behaviour: Scope, Hoisting and TDZ
- **Block scope**: bindings exist only within the nearest `{ ... }` block.  
- **Hoisting**: declarations are hoisted (created) but not initialized — until execution reaches the declaration the binding is in the **Temporal Dead Zone (TDZ)**. Accessing it throws `ReferenceError`.
- **Initialization**: `const` must be initialized at declaration time; `let` can be declared without immediate initialization.

```
{
  console.log(x); // ReferenceError (TDZ)
  let x = 10;
}

const a = 1; // must initialize
// const b; // SyntaxError: Missing initializer in const declaration
```

---

## 2. Examples (basic → advanced)
### Basic use
```
let count = 0;
count = 1; // OK

const PI = 3.14159;
PI = 3; // TypeError: Assignment to constant variable.
```

### Block-scoped loop variables
```
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0); // prints 0 1 2
}
```

### TDZ example with parameters and defaults
```
function foo(x = y, y = 2) { 
  return [x, y];
}
foo(); // ReferenceError: Cannot access 'y' before initialization
```

### Re-declaration rules
```
var x = 1;
var x = 2; // allowed

let y = 1;
let y = 2; // SyntaxError: Identifier 'y' has already been declared

// but different scopes allowed
function f() {
  let y = 1;
  if (true) {
    let y = 2; // different binding
  }
}
```

---

## 3. Under the hood (how engines implement)
- When parsing a block/function, engine creates entries for declared names in an environment record.
- For `let`/`const`, the engine leaves the binding uninitialized until the declaration evaluation step; reading before init => TDZ error.
- `const` is a binding with `immutable` flag; assignment operation checks flag and throws if set.
- Loop constructs with `let` create a new lexical environment per iteration (specification detail), enabling correct closures in loops.

Pseudo-code:
```
enterBlock()
  create environment record
  for each let/const declaration in block:
    createBinding(name, undefined, uninitialized=true)
  execute block statements
  at declaration evaluation: initialize binding with value, uninitialized=false
exitBlock() -> remove bindings
```

---

## 4. Mutability vs binding immutability
- `const` prevents reassignment of the binding, not mutation of the value.
```
const arr = [1,2];
arr.push(3); // ok -> [1,2,3]
arr = [4]; // TypeError
```

- To make deep immutability use patterns/libraries (immer, deepFreeze):
```
function deepFreeze(obj) {
  Object.getOwnPropertyNames(obj).forEach((name) => {
    const prop = obj[name];
    if (typeof prop === 'object' && prop !== null) deepFreeze(prop);
  });
  return Object.freeze(obj);
}
```

---

## 5. Common pitfalls & gotchas
1. **TDZ surprises**: referencing before declaration throws — sometimes caused by hoisted functions/vars.  
2. **Top-level differences**: in browser scripts `var` creates `window.x`, but `let`/`const` do not attach to `window`.  
3. **Mutable reference confusion**: devs expect `const` to be immutable — clarify binding vs value.  
4. **Loop captured variables**: `let` fixes classic closure-in-loop bug but creates a new binding each iteration which may allocate more memory. Use with care in hot loops.

---

## 6. Patterns & Best practices
- Use `const` by default. When you need to reassign, use `let`. Avoid `var` entirely in modern code.  
- Prefer immutable data structures when possible; use `const` along with pure operations that return new objects (functional style).  
- Use `let` for loop counters where the counter is mutated; use `const` for loop bodies that should not reassign the variable.

Examples:
```
const users = fetchUsers();
for (const user of users) { // don't reassign user variable
  process(user);
}

let sum = 0; // mutated within block
for (let i = 0; i < arr.length; i++) {
  sum += arr[i]; 
}
```

---

## 7. Memory & performance notes
- `let` per-iteration binding creates fresh lexical environments in spec. Modern engines optimize this but in extremely hot loops allocating heavy objects per iteration can cost. Reuse objects where possible (object pools) for performance critical paths.

---

## 8. Interview Questions (easy → hard)
1. Explain differences between `var`, `let`, and `const`
**Answer:**
- **`var`**
  - Function-scoped (lives inside the whole function).
  - Can be re-declared and updated.
  - Hoisted to the top of the scope (but set to `undefined`).

- **`let`**
  - Block-scoped (only exists inside `{ ... }`).
  - Can be updated but **not re-declared** in the same scope.
  - Hoisted too, but not usable until execution reaches its line (TDZ).

- **`const`**
  - Block-scoped like `let`.
  - Cannot be reassigned.
  - Must be initialized immediately.

2. What is the Temporal Dead Zone (TDZ)? Provide an example.
**Answer:**
- TDZ is the time between **entering a block** and **actually declaring the variable**.  
- During this time, accessing the variable gives an error.

```js
{
  // TDZ starts here
  console.log(a); // ❌ ReferenceError
  let a = 10;    // declaration ends TDZ
  console.log(a); // ✅ 10
}
```

3. Why does `let` fix the closure-in-loop problem?
**Answer:**
- With `var`, all iterations share the **same variable**.  
- With `let`, each iteration gets a **new variable copy**, so closures “remember” the right value.

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0); 
}
// ❌ prints 3, 3, 3

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0); 
}
// ✅ prints 0, 1, 2
```

4. Can you reassign a `const` object? How to freeze an object?
**Answer:**
- `const` stops **reassignment** (you can’t point to a new object).  
- But the object’s contents **can still change**.  
- To make it unchangeable: use `Object.freeze(obj)`.

```js
const user = { name: "A" };
user.name = "B";   // ✅ allowed
user = { age: 20 } // ❌ not allowed

Object.freeze(user);
user.name = "C";   // ❌ won’t work now
```

5. How do top-level `var` and `const` differ in browser global scope?
**Answer:**
- **`var`**: attaches to `window` object (global).  
- **`const`/`let`**: don’t attach to `window`, live in a separate “script scope”.  

```js
var x = 1;
const y = 2;

console.log(window.x); // ✅ 1
console.log(window.y); // ❌ undefined
```

6. Explain the engine steps for creating/initializing `let` bindings.
**Answer:**
When JavaScript engine sees a `let`:
- **Parse phase**: It knows the variable exists (creates binding).  
- **TDZ**: Until code execution reaches that line, it’s uninitialized.  
- **Initialization**: At the line, memory is assigned and value stored.  

7. Discuss memory/performance trade-offs for `let` in tight loops and how you'd optimize.
**Answer:**
- With `let` inside a loop, each iteration gets a new variable.  
- That means more memory usage (each closure keeps its copy).  
- Usually fine, but in **huge loops**, it could be slower.  

**Optimization tips:**
```js
// Heavy: creates new `i` each loop
for (let i = 0; i < big; i++) { ... }

// Lighter: reuse one variable
let i;
for (i = 0; i < big; i++) { ... }
```
---

## References
- MDN: [let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), [const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
