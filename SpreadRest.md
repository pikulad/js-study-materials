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

ES6 — Spread (`...`) & Rest (`...`)

---

## Summary
The `...` syntax is used both to expand iterables/objects (**spread**) and to collect remaining elements (**rest**). Context (LHS vs RHS) determines the behavior. They are powerful for cloning, merging, variadic functions, and concise transformations.

##### Why Spread (...) was introduced

- Simplifies array copying → Instead of using slice() or loops.
- Simplifies array merging → Instead of using concat().
- Expands arrays into arguments → Replaces Function.prototype.apply().
- Supports object copying/merging (ES2018) → Easier than Object.assign().
- Improves readability → Code is shorter and clearer.

##### Why Rest (...) was introduced

- Handles variable arguments in functions → Replaces the old arguments object.
- Collects extra parameters into an array → Makes functions more flexible.
- Works with array destructuring → Extracts part of an array and keeps the rest.
- Works with object destructuring → Extracts some properties and groups the remaining ones.
- Improves clarity → Clearly shows “the rest” of the values being grouped.

---

## 1. Spread in arrays & iterables
```
const a = [1,2];
const b = [...a, 3]; // [1,2,3]
const s = new Set([1,2]);
const arr = [...s]; // convert iterable to array
```

---

## 2. Spread in objects
```
const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 }; // shallow copy
const obj3 = { b: 99, ...obj1 }; // obj1.b overrides later props -> b=2
```

- Spreading objects copies **own enumerable properties** and invokes getters; property descriptors are not preserved.

---

## 3. Rest for parameters & destructuring
```
function sum(...nums) { 
  return nums.reduce((s,n) => s+n, 0); 
}
const [first, ...rest] = [1,2,3,4]; // rest = [2,3,4]
const { a, ...restObj } = { a:1, b:2, c:3 }; // restObj = { b:2, c:3 }
```

---

## 4. Shallow copy vs deep copy
- Spread performs **shallow copy**. Nested objects are referenced. To deep clone use structuredClone (modern), JSON (limited), or recursive clone libraries.

```
const original = { nested: { x: 1 } };
const copy = { ...original };
copy.nested.x = 2; // original.nested.x === 2
```

---

## 5. Getters, descriptors & prototype
- Spread reads property values (invokes getters). Non-enumerable properties and properties on the prototype chain are ignored. Symbol-keyed properties are also ignored by object spread.

---

## 6. Performance considerations
- Spread creates intermediate arrays/objects; avoid overusing in hot loops. Use manual loops or in-place mutation for performance critical code.

---

## 7. Advanced patterns
### Merge arrays without duplicates
```
const a = [1,2,3];
const b = [1,2,3,4,5,6];
const mergedUnique = [...new Set([...a, ...b])];
console.log(mergedUnique); //[ 1, 2, 3, 4, 5, 6 ]
```

### Variadic compose
```
const max = (...n) => Math.max(...n);
```

### Apply arrays to functions (before spread existed)
`Function.prototype.apply` was used; spread is cleaner and faster in modern engines.

---

## 8. Interview Questions (easy → hard)

##### 1. Explain difference between rest and spread.

- **Spread (`...`)**
  - Expands values into individual elements.
  - Used for:
    - Copying arrays/objects.
    - Merging arrays/objects.
    - Passing array elements as function arguments.
  - Example:
    ```
    let arr = [1, 2, 3];
    let copy = [...arr]; // [1, 2, 3]
    Math.max(...arr);    // 3
    ```

- **Rest (`...`)**
  - Collects multiple values into an array or object.
  - Used for:
    - Capturing function arguments.
    - Gathering “the rest” of values in destructuring.
  - Example:
    ```
    function sum(...nums) {
      return nums.reduce((a, b) => a + b, 0);
    }
    sum(1, 2, 3); // 6

    let [first, ...others] = [10, 20, 30];
    // first = 10, others = [20, 30]
    ```

##### 2. How does object spread interact with getters and property descriptors?

- Object spread (`{...obj}`) copies **own enumerable properties**.
- **Getters and setters are not preserved** → only their computed values are copied.
- **Property descriptors** (writable, configurable, enumerable) are lost.

Example:
```
let obj = { get value() { return Math.random(); } };
let copy = { ...obj };
console.log(copy.value); // fixed value, not a getter anymore
```

Preserve descriptors and accessors:
```
let clone = Object.create(
  Object.getPrototypeOf(obj),
  Object.getOwnPropertyDescriptors(obj)
);
```

##### 3. How to perform a deep clone? Discuss tradeoffs.

- **`JSON.parse(JSON.stringify(obj)`)**
  - ✅ Simple, removes references.
  - ❌ Loses functions, Dates, Maps, Sets, prototypes.

- **`structuredClone(obj)`** (modern JS)
  - ✅ Handles Dates, Maps, Sets, ArrayBuffers, circular references.
  - ❌ Not available in older JS environments.

- **Custom recursive clone**
  - ✅ Full control over cloning rules.
  - ❌ Complex and potentially slower.

**Tradeoffs**:
- JSON: fastest but limited.
- structuredClone: accurate but slower.
- Custom: flexible but complex.


##### 4. Implement a custom `shallowClone` using iteration and descriptor copying. 

```
function shallowClone(obj) {
  let clone = Object.create(Object.getPrototypeOf(obj));
  for (let key of Reflect.ownKeys(obj)) {
    let descriptor = Object.getOwnPropertyDescriptor(obj, key);
    Object.defineProperty(clone, key, descriptor);
  }
  return clone;
}

// Example
let source = { a: 1, get b() { return 2; } };
let copy = shallowClone(source);
console.log(Object.getOwnPropertyDescriptor(copy, 'b'));
```
- This preserves **descriptors, getters, and setters**.


##### 5. Show a performance-sensitive alternative to repeated spread operations in a loop.

- **Problem**: Spread creates a new array/object on each iteration.
```
let result = [];
for (let i = 0; i < 1000; i++) {
  result = [...result, i]; // ❌ inefficient
}
```

- **Better for arrays**:
```
let result = [];
for (let i = 0; i < 1000; i++) {
  result.push(i); // ✅ efficient
}
```

- **Better for objects**:
```
let target = {};
for (let i = 0; i < 1000; i++) {
  Object.assign(target, { [i]: i });
}
```

---

## References
- MDN: [Spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
