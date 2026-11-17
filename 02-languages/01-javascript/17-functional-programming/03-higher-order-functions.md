# Higher-Order Functions

A higher-order function (HOF) is a function that does at least one of the following:

* Accepts another function as an argument.
* Returns another function as its result.

Higher-order functions are an important concept in functional programming and JavaScript because functions are treated as first-class values.

Example:

```javascript
function greet(name) {
  return `Hello, ${name}!`;
}

function processUser(callback) {
  return callback("Alice");
}

console.log(processUser(greet));
```

Output:

```text
Hello, Alice!
```

Here, `processUser()` is a higher-order function because it accepts another function as an argument.

# 🗁 First-Class Functions

JavaScript treats functions as first-class citizens.

This means functions can be:

* Assigned to variables.
* Stored in objects.
* Stored in arrays.
* Passed as arguments.
* Returned from other functions.
* Created dynamically.

**Assigning a function to a variable**

```javascript
function sayHello() {
  return "Hello!";
}

const greet = sayHello;

console.log(greet()); // Hello!
```

**Storing functions in objects**

```javascript
const operations = {
  add: (a, b) => a + b,
  subtract: (a, b) => a - b
};

console.log(operations.add(10, 5));      // 15
console.log(operations.subtract(10, 5)); // 5
```

**Storing functions in arrays**

```javascript
const operations = [
  (a, b) => a + b,
  (a, b) => a - b,
  (a, b) => a * b
];

console.log(operations[0](10, 5)); // 15
console.log(operations[1](10, 5)); // 5
console.log(operations[2](10, 5)); // 50
```

These capabilities allow JavaScript functions to be used as data.

# 🗁 Functions as Arguments

A function can receive another function as an argument.

Example:

```javascript
function calculate(a, b, operation) {
  return operation(a, b);
}

function add(a, b) {
  return a + b;
}

function multiply(a, b) {
  return a * b;
}

console.log(calculate(10, 5, add));      // 15
console.log(calculate(10, 5, multiply)); // 50
```

The `calculate()` function does not need to know the implementation of each operation. It delegates the operation to the supplied function.

This makes the function reusable.

# 🗁 Functions as Return Values

A function can return another function.

Example:

```javascript
function createGreeting() {
  return function (name) {
    return `Hello, ${name}!`;
  };
}

const greet = createGreeting();

console.log(greet("Alice")); // Hello, Alice!
```

The returned function can be stored in a variable and called later.

An arrow-function version:

```javascript
function createGreeting() {
  return (name) => `Hello, ${name}!`;
}

const greet = createGreeting();

console.log(greet("Bob")); // Hello, Bob!
```

# 🗁 Callback Functions

A callback is a function passed to another function so that it can be invoked by that function.

Example:

```javascript
function processData(data, callback) {
  const result = data.toUpperCase();

  callback(result);
}

function displayResult(value) {
  console.log(value);
}

processData("javascript", displayResult);
```

Output:

```text
JAVASCRIPT
```

Here:

* `processData()` is a higher-order function.
* `displayResult()` is a callback function.
* The callback is invoked after the data has been transformed.

Callbacks can be synchronous or asynchronous, depending on when they are executed.

# 🗁 Built-in Higher-Order Functions

JavaScript provides several built-in higher-order functions.

Common examples include:

* `map()`
* `filter()`
* `reduce()`
* `forEach()`
* `find()`
* `findIndex()`
* `some()`
* `every()`
* `sort()`
* `flatMap()`
* `setTimeout()`
* `setInterval()`
* `addEventListener()`

These functions accept callbacks to customize their behavior.

# 🗁 map() as a Higher-Order Function

The `map()` method accepts a callback and applies it to every array element.

Example:

```javascript
const numbers = [1, 2, 3, 4];

const doubled = numbers.map((number) => {
  return number * 2;
});

console.log(doubled);
```

Output:

```javascript
[2, 4, 6, 8]
```

The callback determines how each element is transformed.

# 🗁 filter() as a Higher-Order Function

The `filter()` method accepts a callback that determines which elements should remain in the resulting array.

Example:

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

const evenNumbers = numbers.filter((number) => {
  return number % 2 === 0;
});

console.log(evenNumbers);
```

Output:

```javascript
[2, 4, 6]
```

The callback returns a Boolean value for each element.

# 🗁 reduce() as a Higher-Order Function

The `reduce()` method accepts a callback that combines array elements into a single accumulated result.

Example:

```javascript
const numbers = [10, 20, 30];

const total = numbers.reduce((sum, number) => {
  return sum + number;
}, 0);

console.log(total); // 60
```

The callback receives the accumulated value and the current element.

# 🗁 forEach() as a Higher-Order Function

The `forEach()` method accepts a callback and executes it for every array element.

Example:

```javascript
const users = ["Alice", "Bob", "Charlie"];

users.forEach((user) => {
  console.log(`Welcome, ${user}`);
});
```

Output:

```text
Welcome, Alice
Welcome, Bob
Welcome, Charlie
```

Unlike `map()`, `forEach()` returns `undefined` and is generally used for side effects rather than creating a transformed array.

# 🗁 sort() as a Higher-Order Function

The `sort()` method accepts a comparison function to determine the ordering of elements.

Example:

```javascript
const numbers = [40, 10, 30, 20];

const sorted = numbers.toSorted((a, b) => {
  return a - b;
});

console.log(sorted);
```

Output:

```javascript
[10, 20, 30, 40]
```

The comparison callback controls the sorting behavior.

# 🗁 setTimeout() as a Higher-Order Function

`setTimeout()` accepts a callback that executes after a specified delay.

Example:

```javascript
function greet() {
  console.log("Hello after two seconds!");
}

setTimeout(greet, 2000);
```

The callback is executed asynchronously after the delay.

An inline callback:

```javascript
setTimeout(() => {
  console.log("Task completed!");
}, 1000);
```

# 🗁 addEventListener() as a Higher-Order Function

The `addEventListener()` method accepts a callback that executes when a specified event occurs.

Example:

```javascript
const button = document.querySelector("#submit");

button.addEventListener("click", () => {
  console.log("Button clicked!");
});
```

The callback is invoked when the user clicks the button.

This is an example of event-driven programming using higher-order functions.

# 🗁 Creating Custom Higher-Order Functions

You can create your own higher-order functions to make application logic reusable.

Example:

```javascript
function executeOperation(a, b, operation) {
  return operation(a, b);
}

const add = (a, b) => a + b;
const subtract = (a, b) => a - b;
const multiply = (a, b) => a * b;

console.log(executeOperation(10, 5, add));      // 15
console.log(executeOperation(10, 5, subtract)); // 5
console.log(executeOperation(10, 5, multiply)); // 50
```

The same function supports different operations without changing its implementation.

# 🗁 Creating a Reusable Validation Function

A higher-order function can accept a validation rule.

Example:

```javascript
function validate(value, rule) {
  return rule(value);
}

const isEmail = (value) => {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value);
};

const isLongEnough = (value) => {
  return value.length >= 8;
};

console.log(validate("alice@example.com", isEmail)); // true
console.log(validate("password", isLongEnough));    // true
```

The validation function remains reusable because the actual rule is supplied as a callback.

# 🗁 Returning Configurable Functions

Higher-order functions can generate specialized functions using configuration values.

Example:

```javascript
function createMultiplier(multiplier) {
  return function (number) {
    return number * multiplier;
  };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);
const quadruple = createMultiplier(4);

console.log(double(5));     // 10
console.log(triple(5));     // 15
console.log(quadruple(5));  // 20
```

Each generated function has its own multiplier.

This behavior is closely related to closures.

# 🗁 Higher-Order Functions and Closures

A returned function can access variables from the scope in which it was created.

Example:

```javascript
function createCounter() {
  let count = 0;

  return function () {
    count++;

    return count;
  };
}

const counter = createCounter();

console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

Here:

* `createCounter()` is a higher-order function because it returns another function.
* The returned function forms a closure over `count`.
* The `count` variable remains accessible between calls.

Closures allow higher-order functions to create stateful function instances.

# 🗁 Function Factories

A function factory is a higher-order function that creates and returns specialized functions.

Example:

```javascript
function createLogger(prefix) {
  return function (message) {
    console.log(`[${prefix}] ${message}`);
  };
}

const info = createLogger("INFO");
const error = createLogger("ERROR");

info("Application started");
error("Database connection failed");
```

Output:

```text
[INFO] Application started
[ERROR] Database connection failed
```

Function factories are useful for:

* Logging utilities.
* Configurable validators.
* Event handlers.
* Formatting functions.
* Reusable business logic.

# 🗁 Function Decorators

A decorator in JavaScript can be implemented as a higher-order function that wraps another function to add behavior.

Example:

```javascript
function withLogging(callback) {
  return function (...args) {
    console.log("Function started");

    const result = callback(...args);

    console.log("Function completed");

    return result;
  };
}

function add(a, b) {
  return a + b;
}

const loggedAdd = withLogging(add);

console.log(loggedAdd(10, 20));
```

Output:

```text
Function started
Function completed
30
```

The original function's calculation remains unchanged, while the wrapper adds logging behavior.

This simple example works with synchronous functions. Wrapping asynchronous functions requires additional handling if completion should be logged after the returned Promise settles.

# 🗁 Function Wrapping with Async Functions

A higher-order function can wrap asynchronous functions.

Example:

```javascript
function withTiming(callback) {
  return async function (...args) {
    const start = performance.now();

    try {
      return await callback(...args);
    } finally {
      const end = performance.now();

      console.log(`Execution time: ${end - start}ms`);
    }
  };
}

async function fetchData() {
  const response = await fetch("/api/users");

  return response.json();
}

const timedFetch = withTiming(fetchData);
```

The wrapper measures execution time and preserves the returned result.

Because it is an asynchronous wrapper, it returns a Promise even if the wrapped function itself returns a non-Promise value.

# 🗁 Function Composition

Higher-order functions can be used to build reusable function composition utilities.

Example:

```javascript
function compose(f, g) {
  return function (value) {
    return f(g(value));
  };
}

const double = (number) => number * 2;
const increment = (number) => number + 1;

const doubleAfterIncrement = compose(double, increment);

console.log(doubleAfterIncrement(5)); // 12
```

Execution order:

```text
5
 |
 v
increment(5) -> 6
 |
 v
double(6) -> 12
```

The `compose()` function returns a new function that combines two other functions.

# 🗁 Practical Example: Reusable Data Processing

Consider a list of products:

```javascript
const products = [
  { name: "Laptop", price: 1200, active: true },
  { name: "Mouse", price: 40, active: true },
  { name: "Keyboard", price: 80, active: false },
  { name: "Monitor", price: 300, active: true }
];
```

Create reusable higher-order functions:

```javascript
function filterBy(callback) {
  return function (items) {
    return items.filter(callback);
  };
}

function mapWith(callback) {
  return function (items) {
    return items.map(callback);
  };
}
```

Create specialized transformations:

```javascript
const activeProducts = filterBy(
  (product) => product.active
);

const getProductNames = mapWith(
  (product) => product.name
);
```

Execute the transformations:

```javascript
const result = getProductNames(
  activeProducts(products)
);

console.log(result);
```

Output:

```javascript
["Laptop", "Mouse", "Monitor"]
```

This separates reusable transformation mechanisms from specific business rules.

# 🗁 Benefits of Higher-Order Functions

Higher-order functions provide several benefits:

* Code reuse: common behavior can be implemented once.
* Flexibility: behavior can be supplied dynamically.
* Abstraction: implementation details can be hidden behind reusable interfaces.
* Composition: small functions can be combined into larger operations.
* Separation of concerns: generic logic can be separated from specific behavior.
* Testability: callbacks can be tested independently.
* Configurability: specialized functions can be generated from parameters.

# 🗁 Common Mistakes

**Passing the result instead of the function**

Incorrect:

```javascript
function execute(callback) {
  return callback();
}

function greet() {
  return "Hello!";
}

execute(greet());
```

`greet()` executes immediately, so its returned string is passed instead of the function.

Correct:

```javascript
execute(greet);
```

**Confusing function references with function calls**

```javascript
const callback = greet;
```

This stores a function reference.

```javascript
const result = greet();
```

This executes the function.

**Forgetting to return a value**

```javascript
function doubleNumbers(numbers) {
  numbers.map((number) => number * 2);
}
```

The outer function returns `undefined`.

Correct:

```javascript
function doubleNumbers(numbers) {
  return numbers.map((number) => number * 2);
}
```

**Using map() for side effects**

Avoid using `map()` when the transformed array is not needed.

Prefer `forEach()` for simple side-effect operations.

**Creating unnecessary abstractions**

Not every callback requires a custom higher-order function. Use abstraction when it improves reuse, clarity, or flexibility.

# 🗁 Best Practices

* Use meaningful names for callback parameters.
* Prefer small, focused callback functions.
* Pass function references when execution should happen later.
* Return values explicitly when building reusable transformations.
* Use pure callbacks when possible.
* Keep asynchronous and synchronous behavior clear.
* Avoid unnecessary nesting.
* Use higher-order functions when they genuinely improve abstraction and reuse.
* Document callback expectations, including arguments and return values.
* Preserve important function behavior when creating wrappers.

# 🗁 Summary

Higher-order functions are functions that accept other functions as arguments or return functions as results.

Important concepts include:

* First-class functions.
* Functions as arguments.
* Functions as return values.
* Callback functions.
* Built-in higher-order methods.
* Custom higher-order functions.
* Function factories.
* Closures.
* Function decorators and wrappers.
* Function composition.
* Reusable data-processing pipelines.

Higher-order functions are fundamental to JavaScript's functional programming model and are widely used in array processing, event handling, asynchronous programming, and application architecture.
