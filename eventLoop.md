---
marp: true
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('Assets/background.svg')
---

![bg left:40% 80%](Assets/codeastu_ciklum.png)

# **JavaScript Event Loop**
---

### **What is the Event Loop?**

- JavaScript is **single-threaded**: it executes one task at a time.
- The **Event Loop** allows JS to perform **non-blocking operations** like I/O while still being single-threaded.
- It continuously checks the **Call Stack** and the **Task Queues** (Microtasks & Macrotasks) to execute pending tasks.

Think of it like:

**Call Stack** → Where functions are executed.
**Task Queues** → Where async callbacks wait until the stack is free.
**Event Loop** → The manager that decides what to run next.

--- 

### **Call Stack**

- The **Call Stack** is a data structure that keeps track of function execution.
- Functions are pushed to the stack when called, and popped when completed.
- If a function takes too long, it **blocks the stack**.
---
**Example:**
```javascript
function first() {
  console.log('First');
}
function second() {
  console.log('Second');
}
first();
second();
```

**Output:**

```.js
First
Second
```
---
#### 🔄 Event Loop Flow Diagram

```text
 ┌───────────────────────────┐
 │       Call Stack          │
 │  (Executes functions)     │
 └─────────────▲─────────────┘
               │
               │
     ┌─────────┴─────────┐
     │    Event Loop     │
     │ (Task Manager)    │
     └─────────▲─────────┘
               │
     ┌─────────┼─────────┐
     │                   │
┌────┴─────┐       ┌─────┴─────┐
│ Microtask│       │ Macrotask │
│   Queue  │       │   Queue   │
│(Promises,        │(setTimeout, 
│queueMicrotask)   │setInterval, I/O)
└──────────┘       └───────────┘
```



