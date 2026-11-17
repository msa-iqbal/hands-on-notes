# JavaScript Callback Functions

A **callback function** is a function passed as an argument to another function so that the receiving function can execute it when needed.

Callbacks are an important part of JavaScript because functions are first-class values. They can be assigned to variables, passed as arguments, and executed from other functions.

Callbacks are commonly used in:

* Event handling.
* Array methods.
* Asynchronous operations.
* Timers.
* API requests.
* Higher-order functions.

# 🗁 Basic Syntax

```javascript
function mainFunction(callback) {
  // Perform an operation

  callback();
}

function callbackFunction() {
  // Callback logic
}

mainFunction(callbackFunction);
```

Example:

```javascript
function greet(callback) {
  console.log("Hello!");

  callback();
}

function sayGoodbye() {
  console.log("Goodbye!");
}

greet(sayGoodbye);
```

Output:

```text
Hello!
Goodbye!
```

Here:

* `sayGoodbye` is the callback function.
* `greet` receives the callback as an argument.
* `callback()` executes the supplied function.

Notice that `sayGoodbye` is passed without parentheses. Passing it this way provides the function itself rather than immediately executing it.

# 🗁 Passing an Anonymous Function as a Callback

You do not always need to define a callback separately. You can pass an anonymous function directly.

```javascript
function process(callback) {
  console.log("Processing data...");

  callback();
}

process(function () {
  console.log("Processing completed!");
});
```

Output:

```text
Processing data...
Processing completed!
```

This approach is useful when a callback is short and only needed in one place.

# 🗁 Passing an Arrow Function as a Callback

Arrow functions provide concise callback syntax.

```javascript
function calculate(a, b, callback) {
  const result = a + b;

  callback(result);
}

calculate(10, 20, result => {
  console.log("Result:", result);
});
```

Output:

```text
Result: 30
```

The callback receives the calculated result as an argument.

# 🗁 Callback Functions with Parameters

A callback can receive values from the function that invokes it.

```javascript
function getUser(callback) {
  const user = {
    id: 1,
    name: "Alex",
    role: "Developer"
  };

  callback(user);
}

function displayUser(user) {
  console.log(`Name: ${user.name}`);
  console.log(`Role: ${user.role}`);
}

getUser(displayUser);
```

Output:

```text
Name: Alex
Role: Developer
```

The `getUser()` function creates the user object and passes it to the callback.

This allows the receiving function to decide how the data should be processed.

# 🗁 Callback Functions with Return Values

A callback can return a value, which the receiving function can use.

```javascript
function calculate(a, b, callback) {
  return callback(a, b);
}

function add(x, y) {
  return x + y;
}

const result = calculate(10, 20, add);

console.log(result);
```

Output:

```text
30
```

The `calculate()` function delegates the calculation to the callback and returns its result.

# 🗁 Synchronous Callback Functions

A synchronous callback executes immediately during the execution of the function that invokes it.

Example:

```javascript
function processData(callback) {
  console.log("Step 1: Starting");

  callback();

  console.log("Step 3: Finished");
}

function displayMessage() {
  console.log("Step 2: Processing");
}

processData(displayMessage);
```

Output:

```text
Step 1: Starting
Step 2: Processing
Step 3: Finished
```

The callback completes before the next statement in `processData()` executes.

Array methods such as `forEach()`, `map()`, and `filter()` commonly use synchronous callbacks.

# 🗁 Asynchronous Callback Functions

An asynchronous callback executes later, after an asynchronous operation reaches the point where the callback should be invoked.

JavaScript commonly uses asynchronous callbacks for:

* Timers.
* Network requests.
* Event handling.
* File operations in Node.js.
* I/O operations.

Example using `setTimeout()`:

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Callback executed");
}, 1000);

console.log("End");
```

Output:

```text
Start
End
Callback executed
```

The callback is scheduled to execute after approximately one second.

The synchronous statements continue executing without waiting for the timer callback.

# 🗁 Callback Functions with `setInterval()`

The `setInterval()` function repeatedly executes a callback after a specified interval.

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

The callback executes repeatedly until `clearInterval()` cancels the interval.

# 🗁 Callback Functions with Array Methods

JavaScript provides several built-in array methods that accept callback functions.

**Using `forEach()`:**

Executes a callback for each array element.

```javascript
const numbers = [10, 20, 30];

numbers.forEach(number => {
  console.log(number);
});
```

Output:

```text
10
20
30
```

**Using `map()`:**

Creates a new array by transforming each element.

```javascript
const numbers = [1, 2, 3, 4];

const doubled = numbers.map(number => number * 2);

console.log(doubled);
```

Output:

```text
[2, 4, 6, 8]
```

**Using `filter()`:**

Creates a new array containing elements that satisfy a condition.

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

const evenNumbers = numbers.filter(number => number % 2 === 0);

console.log(evenNumbers);
```

Output:

```text
[2, 4, 6]
```

**Using `reduce()`:**

Processes array elements and accumulates them into a single result.

```javascript
const numbers = [10, 20, 30];

const total = numbers.reduce((sum, number) => sum + number, 0);

console.log(total);
```

Output:

```text
60
```

# 🗁 Callback Functions with Event Listeners

Event listeners use callbacks to execute code when a particular event occurs.

```javascript
const button = document.querySelector("#submit");

button.addEventListener("click", function () {
  console.log("Button clicked!");
});
```

The callback is not executed when `addEventListener()` is registered. It executes when the browser dispatches the matching event.

You can also use an arrow function:

```javascript
button.addEventListener("click", () => {
  console.log("Button clicked!");
});
```

The event object can be received as a callback parameter.

```javascript
button.addEventListener("click", event => {
  console.log("Event type:", event.type);
  console.log("Target:", event.target);
});
```

# 🗁 Callback Functions with Fetch API

The Fetch API returns a Promise, but callbacks can still be used to process its results.

```javascript
function getUsers(callback) {
  fetch("https://jsonplaceholder.typicode.com/users")
    .then(response => response.json())
    .then(users => callback(users))
    .catch(error => console.error(error));
}

function displayUsers(users) {
  users.forEach(user => {
    console.log(user.name);
  });
}

getUsers(displayUsers);
```

Here:

* `getUsers()` retrieves user data.
* The Promise handlers process the response.
* `displayUsers()` receives the resulting array through a callback.

This demonstrates how callbacks can be used together with Promises.

# 🗁 Error-First Callback Pattern

In traditional Node.js callback-based APIs, a common convention is the **error-first callback pattern**.

The first callback argument represents an error, and the following arguments contain successful results.

```javascript
function readData(callback) {
  const error = null;
  const data = "File content";

  callback(error, data);
}

readData((error, data) => {
  if (error) {
    console.error("Error:", error);
    return;
  }

  console.log("Data:", data);
});
```

Output:

```text
Data: File content
```

When an error occurs, the callback receives the error as its first argument.

```javascript
function readData(callback) {
  const error = new Error("Unable to read file");

  callback(error, null);
}

readData((error, data) => {
  if (error) {
    console.error(error.message);
    return;
  }

  console.log(data);
});
```

Output:

```text
Unable to read file
```

This convention makes error handling consistent across callback-based APIs.

# 🗁 Callback Functions and Execution Order

The order in which callbacks execute depends on whether they are invoked synchronously or scheduled asynchronously.

Consider:

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

Even with a delay of `0`, the timer callback does not execute immediately.

The callback is scheduled through the timer mechanism and runs when the JavaScript execution stack is available and the event loop processes the timer task.

A zero-delay timer means the callback becomes eligible to run after the delay threshold, not that it runs immediately.

# 🗁 Callback Hell

When multiple asynchronous operations depend on one another, callbacks can become deeply nested.

This situation is commonly called **callback hell** or the **Pyramid of Doom**.

```javascript
getUser(userId, function (user) {
  getOrders(user.id, function (orders) {
    getOrderDetails(orders[0].id, function (details) {
      getPayment(details.paymentId, function (payment) {
        console.log(payment);
      });
    });
  });
});
```

As nesting increases, code becomes harder to:

* Read.
* Maintain.
* Debug.
* Handle errors consistently.
* Reuse.

Common solutions include:

* Extracting callbacks into named functions.
* Using Promises.
* Using `async` and `await`.

Callback hell and its solutions are covered in the dedicated asynchronous programming topics.

# 🗁 Practical Example: Custom Calculator

Callbacks allow a function to execute different operations without changing its internal implementation.

```javascript
function calculator(a, b, operation) {
  return operation(a, b);
}

const add = (a, b) => a + b;

const subtract = (a, b) => a - b;

const multiply = (a, b) => a * b;

const divide = (a, b) => {
  if (b === 0) {
    throw new Error("Cannot divide by zero");
  }

  return a / b;
};

console.log(calculator(10, 5, add));
console.log(calculator(10, 5, subtract));
console.log(calculator(10, 5, multiply));
console.log(calculator(10, 5, divide));
```

Output:

```text
15
5
50
2
```

The calculator delegates the operation to the supplied callback.

This makes the function flexible and reusable.

# 🗁 Callback Functions vs Higher-Order Functions

Callbacks and higher-order functions are closely related, but they describe different concepts.

A **callback function** is a function passed to another function.

A **higher-order function** is a function that accepts another function as an argument, returns a function, or both.

Example:

```javascript
function execute(callback) {
  callback();
}

function sayHello() {
  console.log("Hello!");
}

execute(sayHello);
```

In this example:

* `sayHello` is the callback function.
* `execute` is a higher-order function because it accepts a function as an argument.

Every function passed as a callback participates in a higher-order function relationship, but the terms describe different roles.

# 🗁 Common Mistakes

**Calling the callback instead of passing it**

Incorrect:

```javascript
function greet(callback) {
  callback();
}

function sayHello() {
  console.log("Hello!");
}

greet(sayHello());
```

Here, `sayHello()` executes immediately, and its return value is passed to `greet()`.

Correct:

```javascript
greet(sayHello);
```

Pass the function reference when the receiving function is responsible for invoking it.

**Invoking a callback without checking its availability**

```javascript
function process(callback) {
  callback();
}

process();
```

This throws a `TypeError` because `callback` is `undefined`.

If a callback is optional, validate it or provide an appropriate default.

```javascript
function process(callback) {
  if (typeof callback === "function") {
    callback();
  }
}

process();
```

**Forgetting to pass callback arguments**

```javascript
function calculate(callback) {
  const result = 10 + 20;

  callback();
}

calculate(result => {
  console.log(result);
});
```

Output:

```text
undefined
```

Correct:

```javascript
function calculate(callback) {
  const result = 10 + 20;

  callback(result);
}
```

**Confusing synchronous and asynchronous callbacks**

A callback is not automatically asynchronous. Its execution timing depends on how the receiving function invokes or schedules it.

**Assuming asynchronous callbacks execute immediately**

A timer callback scheduled with `setTimeout()` executes after the current synchronous execution completes and the event loop processes its task.

# 🗁 Best Practices

* Use meaningful callback function names.
* Prefer arrow functions for short callbacks when their lexical `this` behavior is appropriate.
* Extract complex callbacks into named functions.
* Validate optional callbacks before invoking them.
* Pass errors consistently in callback-based APIs.
* Avoid deeply nested asynchronous callbacks.
* Use Promises or `async`/`await` for complex asynchronous workflows.
* Keep callback responsibilities focused.
* Understand whether an API invokes callbacks synchronously or asynchronously.
* Avoid unnecessary callbacks when a direct function call is sufficient.

# 🗁 Key Takeaways

* A callback is a function passed as an argument to another function.
* The receiving function decides when to execute the callback.
* Callbacks can be synchronous or asynchronous.
* Callbacks can receive arguments and return values.
* Array methods such as `map()`, `filter()`, and `reduce()` use callbacks.
* Event listeners and timers commonly use callback functions.
* Node.js traditionally uses the error-first callback convention.
* Excessive nested callbacks can lead to callback hell.
* Higher-order functions accept or return functions.
* Callbacks are a fundamental building block of JavaScript's asynchronous programming model.
