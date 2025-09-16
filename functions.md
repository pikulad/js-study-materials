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

Functions

---

### 1. Write a function which returns another function and execute it after calling

- Functions can be declared, executed or returned inside another function
- Functions can also be stored in variables like other values in JavaScript
---
```js
function higherOrderFunction() {
  function displayHello() {
    console.log("Hello");
  }
  return displayHello;
}
// driver code
var func = higherOrderFunction();
func(); // Hello
```

**References**
- https://eloquentjavascript.net/05_higher_order.html
- https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function

---

### 2. Write a function which executes another function received as an argument

- Functions can be passed as arguments to another function
- Passing the function as an argument will pass its reference hence no parenthesis
---
```js
function callbackExecutor(callback) {
  if (typeof callback === "function") {
    callback();
  }
}

// driver code
function callbackFunc() {
  console.log("Callback function executed");
}

callbackExecutor(callbackFunc); // Callback function executed
```

**References**
- https://developer.mozilla.org/en-US/docs/Glossary/Callback_function

---

### 3. Create a function having no parameters declared and print all the arguments passed to it

- When a function is invoked the arguments passed to it are accessible using the default object called "arguments"
- Numbers starting from 0 are set as keys of the object "arguments" corresponding to each argument in the order
- The `arguments` object will have a length property as well which gives count of arguments passed
---
```js
function func() {
  for (let key in arguments) {
    console.log(arguments[key]);
  }
}

// driver code
func(1, "Hello", true);
```

**Notes**
Though the keys of arguments object look like numbers, "arguments" is not an array. Arrow functions will not have arguments object

---
**References**
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments

---

### 4. Write a function which executes only if the number of arguments match the number of parameters the function is expecting

- The number of parameters declared in a function can be obtained by accessing the length property of the function

---
```js
function func(a, b, c) {
  if (func.length === arguments.length) {
    console.log("Number of arguments passed match the expected arguments");
  } else {
    throw new Error(
      "Number of arguments passed do not match the expected arguments"
    );
  }
}
```

**References**
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/length

---

### 5. Design a function which can receive a variable number of arguments in parameters and print them

```js
function varArgsFunc(...params) {
  params.forEach(function (value, index) {
    console.log(index, ": ", value);
  });
}
// driver code
varArgsFunc("Hello", ",", "World", "!!!");
```
**References**
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters

---

### 6. Show the most common ways of creating functions in JavaScript

- Functions are most commonly created using function statements, function expressions and arrow functions
- Function statements get hoisted unlike function expressions

```js
// Regular function as a function statement
function functionName(params) {
  //code block
}
```
---
```js
// Regular function as a function expression
const functionName = function (params) {
  //code block
};
```

**Notes**
As the arrow functions are not verbose, majority of developers prefer to create arrow functions for quick coding

**References**
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions

---

### 6b. Arrow Functions

```js
// Arrow function as a function expression
const arrowFunctionName = (params) => {
  //code block
};
```

---

### 7. Code the different forms of arrow functions for varying number of parameters

- Arrow functions provide simpler syntax over `function` keyword functions
- Arrow functions with single parameter, round brackets are optional
- Arrow functions with single statement with return, flower brackets and return keywords are optional
---

```js
const noArgsFunc = () => {
  return "No args passed";
};

const singleArgFunc = (arg1) => "Argument is " + arg1;

const singleArgFunc2 = (arg1) => {
  console.log("Argument is " + arg1);
  return arg1;
};

const twoArgsFunc = (arg1, arg2) => {
  return arg1 + arg2;
};

const threeArgsFunc = (arg1, arg2, arg3) => {
  console.log("Sum is " + (arg1 + arg2 + arg3));
  return true;
};
```
---
**Notes**
Arrow functions are also called fat arrow functions

**References**
- https://developer.mozilla.org/en-US/docs/web/JavaScript/Reference/Operators/function

---
