# JavaScript Event Loop — Complete Notes
Understanding how JavaScript manages synchronous and asynchronous code using the Call Stack, Web APIs, Microtask Queue, Callback Queue, and Event Loop.

# 📚 Table of Contents

1. What is the Event Loop?
2. Why is the Event Loop Required?
3. JavaScript Runtime
4. Call Stack
5. Web APIs
6. Callback Queue
7. Microtask Queue
8. Event Loop Process
9. Execution Example
10. Complete Example
11. Key Points

---

# 1. What is the Event Loop?

JavaScript is single-threaded, which means it executes one piece of JavaScript code at a time.

However, JavaScript can still handle operations that take time, such as:

- "setTimeout()"
- "setInterval()"
- "fetch()"
- DOM events
- File operations in Node.js

The Event Loop coordinates these operations so JavaScript can continue running other code instead of waiting.

---

# 2. Why is the Event Loop Required?

Consider:
```
console.log("Start");

setTimeout(() => {
  console.log("Hello");
}, 3000);

console.log("End");
```
The browser does not stop JavaScript execution for 3 seconds.

Instead:
```
Start
End
(wait 3 seconds)
Hello
```
The timer is handled separately while JavaScript continues with the remaining synchronous code.

This is one of the main reasons JavaScript applications can remain responsive and non-blocking.

---

# 3. JavaScript Runtime

The JavaScript runtime can be understood using these major parts:
```
             JavaScript Runtime
                    │
             ┌──────▼──────┐
             │ Call Stack  │
             └──────┬──────┘
                    │
                    ▼
               Web APIs
                    │
                    ▼
            Callback / Task
                 Queue

          Microtask Queue
                    ▲
                    │
               Event Loop
```
Each component has a different responsibility during program execution.

---

# 4. Call Stack

The Call Stack is the area where JavaScript keeps track of currently executing functions.

It follows the LIFO rule:

«Last In, First Out»

Example:
```
function first() {
  second();
}

function second() {
  console.log("Hello");
}

first();
```
Execution:
```
first()
   ↓
second()
   ↓
console.log("Hello")
   ↓
second() removed
   ↓
first() removed
```
Synchronous JavaScript runs directly through the Call Stack.

---

# 5. Web APIs

Web APIs are features supplied by the browser that can handle operations outside the JavaScript Call Stack.

Examples include:

- Timers
- "fetch()"
- DOM events
- Geolocation
- Browser storage APIs

For example:
```
setTimeout(() => {
  console.log("Finished");
}, 2000);
```
Conceptually:
```
JavaScript
    ↓
setTimeout()
    ↓
Browser Timer
    ↓
Timer completes
    ↓
Callback becomes eligible
```
The JavaScript thread can continue executing other code while the timer is running.

---

# 6. Callback Queue / Task Queue

When certain asynchronous operations finish, their callbacks become tasks waiting to be processed.

For example:
```
setTimeout(() => {
  console.log("Timer completed");
}, 1000);
```
After the timer expires, its callback waits in the Task Queue until it is allowed to run.

The callback does not automatically interrupt currently running JavaScript.

---

# 7. Microtask Queue

The Microtask Queue contains callbacks that need to be processed with higher priority than normal task-queue callbacks.

Common examples:

- Promise ".then()"
- Promise ".catch()"
- Promise ".finally()"
- "queueMicrotask()"
- "MutationObserver"

Example:
```
Promise.resolve().then(() => {
  console.log("Promise executed");
});
```
The callback goes into the Microtask Queue.

A simplified priority model is:
```
Synchronous Code
       ↓
Microtask Queue
       ↓
Task / Callback Queue
```
After the current synchronous work finishes, pending microtasks are processed before moving on to the next ordinary task.

---

# 8. Event Loop Process

The Event Loop coordinates when waiting asynchronous callbacks can execute.

A simplified flow is:
```
Start
  ↓
Execute current JavaScript
  ↓
Is the Call Stack empty?
  ↓
Process pending Microtasks
  ↓
Are Microtasks finished?
  ↓
Run an eligible Task
  ↓
Repeat
```
The important point is that microtasks are drained before the runtime moves to the next task.

---

# 9. Execution Example

Consider:
```
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

Promise.resolve().then(() => {
  console.log("C");
});

console.log("D");
```
## Step 1

console.log("A");

## Output:

A

## Step 2

"setTimeout()" registers a timer.

Its callback will become a task after the timer is ready.

## Step 3

The Promise callback is placed in the Microtask Queue.

## Step 4

console.log("D");

Output:

A
D

## Step 5

The current synchronous work finishes.

The Event Loop processes the Microtask Queue first.

So:

C

is printed.

## Step 6

After the microtask queue is empty, the timer callback can run.

Final output:

A
D
C
B

---

# 10. Complete Example

console.log("1. Start");
```
setTimeout(() => {
  console.log("2. Timer");
}, 0);

Promise.resolve().then(() => {
  console.log("3. Promise");
});

console.log("4. End");
```
Output

1. Start
4. End
3. Promise
2. Timer

Why?

- ""1. Start"" is synchronous.
- "setTimeout()" registers a timer.
- The Promise callback enters the Microtask Queue.
- ""4. End"" is synchronous.
- Current JavaScript execution finishes.
- The Promise microtask runs first.
- The timer task runs afterward.

---


# 11. Key Points

- JavaScript executes JavaScript code on a single main thread.
- The Call Stack handles active JavaScript execution.
- Browser APIs can handle asynchronous operations.
- Completed asynchronous callbacks become eligible for queued execution.
- Promise callbacks are placed in the Microtask Queue.
- Microtasks are processed before the next ordinary task.
- "setTimeout(..., 0)" does not mean immediate execution.
- The Event Loop coordinates the execution of queued work.
- This mechanism allows JavaScript applications to handle asynchronous operations without blocking the main JavaScript execution.

#  👨‍💻 Author
Latesh Padaliya

🎓 B.Tech Computer Science Engineering Student

🌱 Aspiring Full Stack Developer

GitHub: https://github.com/LateshDev

LinkedIn: https://www.linkedin.com/in/latesh-padaliya

⭐ Support
If you like this project, consider giving it a ⭐ on GitHub.
