# Callbacks

A callback is a function passed as an argument to another function so that it can be executed later or at a specific point during the execution of that function.

Callbacks are one of the fundamental concepts in JavaScript. They are commonly used for:

* Asynchronous operations.
* Event handling.
* Array iteration and transformation.
* Timers.
* Custom function behavior.
* Higher-order functions.

Understanding callbacks is essential for working with JavaScript's asynchronous programming model.

# 🗁 What Is a Callback Function?

A callback function is simply a function passed to another function as an argument.

Example:

```javascript
function greet(name, callback) {
  console.log(`Hello, ${name}`);

  callback();
}

function sayGoodbye() {
  console.log("Goodbye!");
}

greet("John", sayGoodbye);
```

Output:

```text
Hello, John
Goodbye!
```

Explanation:

* `greet()` accepts two parameters.
* `name` receives the value `"John"`.
* `callback` receives the `sayGoodbye` function.
* `callback()` executes the function passed to `greet()`.

The `sayGoodbye` function is a callback because it is passed to another function.

# 🗁 Function as an Argument

JavaScript treats functions as first-class values.

This means functions can be:

* Assigned to variables.
* Passed as arguments.
* Returned from other functions.
* Stored in objects and arrays.

Example:

```javascript
function add(a, b) {
  return a + b;
}

function calculate(a, b, operation) {
  return operation(a, b);
}

const result = calculate(10, 20, add);

console.log(result);
```

Output:

```text
30
```

Here, `add` is passed as a callback to `calculate()`.

# 🗁 Callback vs Function Call

There is an important difference between passing a function and calling it.

**Passing a function reference**

```javascript
function greet() {
  console.log("Hello");
}

function execute(callback) {
  callback();
}

execute(greet);
```

The function reference is passed to `execute()`.

**Calling a function immediately**

```javascript
execute(greet());
```

Here, `greet()` executes immediately, and its return value is passed to `execute()`.

If the intention is to pass the function itself, do not add parentheses.

# 🗁 Synchronous Callbacks

A synchronous callback executes immediately during the execution of the function that receives it.

Example:

```javascript
function processUser(name, callback) {
  console.log("Processing user");

  callback(name);

  console.log("Processing completed");
}

function displayUser(name) {
  console.log(`User: ${name}`);
}

processUser("John", displayUser);
```

Output:

```text
Processing user
User: John
Processing completed
```

The callback executes before `processUser()` finishes.

# 🗁 Array Method Callbacks

JavaScript array methods frequently accept callback functions.

These methods use callbacks to define how each array element should be processed.

**forEach()**

Executes a callback for each array element.

```javascript
const numbers = [1, 2, 3, 4];

numbers.forEach(number => {
  console.log(number);
});
```

Output:

```text
1
2
3
4
```

**map()**

Creates a new array by applying a callback to each element.

```javascript
const numbers = [1, 2, 3];

const doubled = numbers.map(number => number * 2);

console.log(doubled);
```

Output:

```text
[2, 4, 6]
```

**filter()**

Creates a new array containing elements that satisfy the callback condition.

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

const evenNumbers = numbers.filter(number => number % 2 === 0);

console.log(evenNumbers);
```

Output:

```text
[2, 4, 6]
```

**find()**

Returns the first element that satisfies the callback condition.

```javascript
const users = [
  { id: 1, name: "John" },
  { id: 2, name: "Sarah" },
  { id: 3, name: "David" }
];

const user = users.find(item => item.id === 2);

console.log(user);
```

Output:

```javascript
{ id: 2, name: "Sarah" }
```

# 🗁 Asynchronous Callbacks

An asynchronous callback is executed later, after an asynchronous operation reaches the appropriate execution point.

The callback is usually scheduled by a runtime API and executed when the call stack is available.

Common examples include:

* `setTimeout()`.
* `setInterval()`.
* DOM event listeners.
* Node.js callback-based APIs.

# 🗁 setTimeout() Callback

`setTimeout()` accepts a callback function and schedules it to execute after a specified delay.

Example:

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Callback executed");
}, 2000);

console.log("End");
```

Output:

```text
Start
End
Callback executed
```

Explanation:

* `"Start"` executes immediately.
* `setTimeout()` schedules the callback.
* `"End"` executes without waiting for the timer.
* The callback becomes eligible to execute after approximately two seconds.

The callback does not execute inside the original synchronous function call.

# 🗁 setInterval() Callback

`setInterval()` repeatedly schedules a callback at a specified interval.

Example:

```javascript
let count = 0;

const intervalId = setInterval(() => {
  count++;

  console.log(`Count: ${count}`);

  if (count === 3) {
    clearInterval(intervalId);
  }
}, 1000);
```

Output:

```text
Count: 1
Count: 2
Count: 3
```

The callback executes repeatedly until `clearInterval()` stops the interval.

# 🗁 Event Listener Callbacks

Event listeners use callbacks to respond to browser events.

Example:

```javascript
const button = document.querySelector("#submit");

button.addEventListener("click", () => {
  console.log("Button clicked");
});
```

The callback is not executed when `addEventListener()` is called.

It executes when the specified event occurs.

Another example:

```javascript
document.addEventListener("keydown", event => {
  console.log(`Key pressed: ${event.key}`);
});
```

The callback receives an event object containing information about the keyboard event.

# 🗁 Callback Parameters

A callback can receive arguments from the function that executes it.

Example:

```javascript
function processNumber(number, callback) {
  const result = number * 2;

  callback(result);
}

function displayResult(value) {
  console.log(`Result: ${value}`);
}

processNumber(10, displayResult);
```

Output:

```text
Result: 20
```

The `processNumber()` function controls when the callback executes and what value it receives.

# 🗁 Callback with Multiple Parameters

Callbacks can receive multiple arguments.

Example:

```javascript
function calculate(a, b, callback) {
  const sum = a + b;
  const difference = a - b;

  callback(sum, difference);
}

function displayResults(sum, difference) {
  console.log("Sum:", sum);
  console.log("Difference:", difference);
}

calculate(20, 10, displayResults);
```

Output:

```text
Sum: 30
Difference: 10
```

# 🗁 Anonymous Callback Functions

An anonymous function is a function without a declared name.

It can be passed directly as a callback.

Example:

```javascript
function execute(callback) {
  callback();
}

execute(function () {
  console.log("Anonymous callback executed");
});
```

Arrow functions are also commonly used:

```javascript
execute(() => {
  console.log("Arrow callback executed");
});
```

Anonymous callbacks are useful when a function is needed only at a particular location.

# 🗁 Named Callback Functions

A named callback is a separately declared function passed to another function.

Example:

```javascript
function showMessage() {
  console.log("Hello from callback");
}

function execute(callback) {
  callback();
}

execute(showMessage);
```

Named callbacks are useful when:

* The callback contains substantial logic.
* The same callback is reused.
* The function needs independent testing.
* A meaningful function name improves readability.

# 🗁 Callback Return Values

The return value of a callback can be used by the function that executes it.

Example:

```javascript
function calculate(a, b, callback) {
  return callback(a, b);
}

function multiply(a, b) {
  return a * b;
}

const result = calculate(5, 4, multiply);

console.log(result);
```

Output:

```text
20
```

The `calculate()` function returns the value returned by the callback.

However, not every callback's return value is used.

For example, `forEach()` ignores callback return values.

# 🗁 Callback-Based API Design

Callbacks allow functions to delegate specific behavior to their callers.

Example:

```javascript
function processOrder(order, onSuccess, onFailure) {
  if (!order) {
    onFailure("Invalid order");
    return;
  }

  onSuccess(order);
}

function handleSuccess(order) {
  console.log("Order processed:", order.id);
}

function handleFailure(error) {
  console.error("Order failed:", error);
}

processOrder(
  { id: 101 },
  handleSuccess,
  handleFailure
);
```

Output:

```text
Order processed: 101
```

The function does not need to know how success or failure should be displayed. It delegates those responsibilities through callbacks.

# 🗁 Error-First Callbacks

Error-first callbacks are a common convention in Node.js callback-based APIs.

The first callback argument represents an error, and the following arguments contain successful results.

General structure:

```javascript
function callback(error, result) {
  if (error) {
    console.error(error);

    return;
  }

  console.log(result);
}
```

Example:

```javascript
function divide(a, b, callback) {
  if (b === 0) {
    callback(new Error("Cannot divide by zero"));

    return;
  }

  callback(null, a / b);
}

divide(10, 2, (error, result) => {
  if (error) {
    console.error(error.message);

    return;
  }

  console.log(result);
});
```

Output:

```text
5
```

The convention is:

* First argument: error or `null`.
* Remaining arguments: successful result values.

A simplified file-reading example using Node.js:

```javascript
const fs = require("node:fs");

fs.readFile("example.txt", "utf8", (error, data) => {
  if (error) {
    console.error(error);

    return;
  }

  console.log(data);
});
```

The callback receives either an error or the file contents.

# 🗁 Callback Execution Order

Callbacks do not all execute at the same time.

Their execution depends on whether they are synchronous or asynchronous and how they are scheduled.

Example:

```javascript
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

console.log("C");
```

Output:

```text
A
C
B
```

Even with a zero-millisecond delay, the timer callback does not execute immediately.

It must wait until the current synchronous execution completes and the runtime schedules the callback.

# 🗁 Callback Scope and Closures

Callbacks can access variables from their surrounding lexical scope.

This behavior is possible because JavaScript functions form closures.

Example:

```javascript
function createCounter() {
  let count = 0;

  return function () {
    count++;

    console.log(count);
  };
}

const increment = createCounter();

setTimeout(increment, 1000);
setTimeout(increment, 2000);
setTimeout(increment, 3000);
```

Output:

```text
1
2
3
```

The callback retains access to the `count` variable even after `createCounter()` has finished executing.

# 🗁 Common Callback Mistakes

**Calling the callback instead of passing it**

Incorrect:

```javascript
function greet() {
  console.log("Hello");
}

setTimeout(greet(), 1000);
```

`greet()` executes immediately, and its return value is passed to `setTimeout()`.

Correct:

```javascript
setTimeout(greet, 1000);
```

**Passing arguments incorrectly**

Incorrect:

```javascript
function greet(name) {
  console.log(`Hello ${name}`);
}

setTimeout(greet("John"), 1000);
```

Correct:

```javascript
setTimeout(() => {
  greet("John");
}, 1000);
```

Modern JavaScript timer APIs also support additional arguments:

```javascript
setTimeout(greet, 1000, "John");
```

**Forgetting to check errors**

Incorrect:

```javascript
fs.readFile("example.txt", "utf8", (error, data) => {
  console.log(data);
});
```

Correct:

```javascript
fs.readFile("example.txt", "utf8", (error, data) => {
  if (error) {
    console.error(error.message);

    return;
  }

  console.log(data);
});
```

**Assuming callbacks always execute asynchronously**

Incorrect assumption:

```javascript
function execute(callback) {
  callback();
}

execute(() => {
  console.log("Callback");
});

console.log("Finished");
```

Output:

```text
Callback
Finished
```

A callback is not automatically asynchronous. The function receiving it determines when and how it is executed.

# 🗁 Advantages of Callbacks

* Enable reusable function behavior.
* Support event-driven programming.
* Allow asynchronous operations to notify completion.
* Encourage separation of responsibilities.
* Work naturally with array methods.
* Support flexible API design.
* Are useful for handling user interactions.

# 🗁 Disadvantages of Callbacks

* Deeply nested callbacks can become difficult to maintain.
* Error handling can become repetitive.
* Complex asynchronous workflows can be difficult to coordinate.
* Execution order may become harder to understand.
* Callback-based APIs can be less convenient to compose than Promise-based APIs.

The nested callback problem is commonly known as callback hell and is covered in the next documentation.

# 🗁 Best Practices

* Use descriptive names for reusable callback functions.
* Keep callbacks small and focused.
* Pass function references instead of accidentally invoking functions.
* Handle errors explicitly.
* Use error-first conventions when designing Node.js-style callback APIs.
* Avoid unnecessary callback nesting.
* Prefer Promises for complex asynchronous workflows.
* Use `async` and `await` when they improve readability.
* Understand whether a callback executes synchronously or asynchronously.
* Avoid sharing mutable state unnecessarily between callbacks.

# 🗁 Key Takeaways

* A callback is a function passed as an argument to another function.
* JavaScript supports callbacks because functions are first-class values.
* Callbacks can be synchronous or asynchronous.
* Array methods such as `map()`, `filter()`, and `forEach()` use callbacks.
* Timers and event listeners commonly use asynchronous callbacks.
* Callbacks can receive arguments and return values.
* Error-first callbacks are a common Node.js convention.
* Callbacks can access surrounding variables through closures.
* Nested asynchronous callbacks can lead to callback hell.
* Promises provide a more composable approach to asynchronous programming.
