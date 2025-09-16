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

Scopes

---
# Scopes

Scope defines where variables are accessible.

#### Types of scope:

Global Scope – accessible everywhere.

Function Scope – variables declared with var inside functions.

Block Scope – let and const inside {}.

---
### Examples:
```js
// Global Scope
var globalVar = "I am global";

function testFunction() {
  // Function Scope
  var funcVar = "I am function scoped";
  console.log(globalVar); // ✅ Accessible
}

testFunction();
// console.log(funcVar); // ❌ Error: not accessible outside

// Block Scope
{
  let blockVar = "I am block scoped";
  const blockConst = "Me too!";
  console.log(blockVar);   // ✅ Accessible here
  console.log(blockConst); // ✅ Accessible here
}
// console.log(blockVar);   // ❌ Error
```
---

| Scope Type     | Description | Example |
|----------------|-------------|---------|
| Global Scope   | Accessible everywhere | `var x = 10;` |
| Function Scope | Declared inside a function (with `var`) | `function test(){ var y = 20; }` |
| Block Scope    | Declared inside `{}` with `let` or `const` | `{ let z = 30; }` |