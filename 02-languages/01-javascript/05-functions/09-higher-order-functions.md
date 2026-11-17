# Higher-Order Functions

A **higher-order function** is a function that does at least one of the following:

* Accepts another function as an argument.
* Returns another function as its result.

In JavaScript, functions are first-class objects. This means they can be assigned to variables, passed as arguments, returned from other functions, and stored in data structures.

Higher-order functions use this capability to create reusable, flexible, and composable logic.

# 🗁 Understanding First-Class Functions

JavaScript treats functions like other values.

```javascript
function greet() {
  return "Hello!";
}

const sayHello = greet;

console.log(sayHello()); // Hello!
```

The function is assigned to another variable without being called.

This works because functions are first-class citizens in JavaScript.

# 🗁 Functions as Arguments

A higher-order function can receive another function as an argument.

```javascript
function greet(name) {
  return `Hello, ${name}!`;
}

function processUser(callback) {
  const name = "Muhammad";

  return callback(name);
}

console.log(processUser(greet));
// Hello, Muhammad!
```

Here:

* `processUser()` is the higher-order function.
* `greet()` is passed as an argument.
* `callback` references the passed function.
* `callback(name)` executes that function.

# 🗁 Functions as Return Values

A function can return another function.

```javascript
function createMultiplier(multiplier) {
  return function (number) {
    return number * multiplier;
  };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15
```

The returned functions retain access to the variables from their enclosing scope. This behavior is based on closures.

Arrow functions can make this pattern more concise:

```javascript
const createMultiplier = (multiplier) => {
  return (number) => number * multiplier;
};

const double = createMultiplier(2);

console.log(double(10)); // 20
```

# 🗁 Built-in Higher-Order Functions

JavaScript provides several built-in higher-order functions, especially for working with arrays.

**`map()`**

Transforms every element and returns a new array.

```javascript
const numbers = [1, 2, 3, 4];

const doubled = numbers.map((number) => number * 2);

console.log(doubled);
// [2, 4, 6, 8]
```

**`filter()`**

Returns a new array containing elements that satisfy a condition.

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

const evenNumbers = numbers.filter((number) => {
  return number % 2 === 0;
});

console.log(evenNumbers);
// [2, 4, 6]
```

**`reduce()`**

Processes array elements and accumulates them into a single result.

```javascript
const numbers = [1, 2, 3, 4];

const total = numbers.reduce((sum, number) => {
  return sum + number;
}, 0);

console.log(total); // 10
```

**`forEach()`**

Executes a callback for each array element.

```javascript
const users = ["Alice", "Bob", "Charlie"];

users.forEach((user) => {
  console.log(user);
});
```

**`sort()`**

Accepts a comparison function to control sorting behavior.

```javascript
const numbers = [40, 5, 100, 25];

numbers.sort((a, b) => a - b);

console.log(numbers);
// [5, 25, 40, 100]
```

# 🗁 Creating Custom Higher-Order Functions

Higher-order functions allow developers to separate reusable operations from specific business logic.

```javascript
function calculate(a, b, operation) {
  return operation(a, b);
}

const add = (a, b) => a + b;
const subtract = (a, b) => a - b;
const multiply = (a, b) => a * b;

console.log(calculate(10, 5, add));      // 15
console.log(calculate(10, 5, subtract)); // 5
console.log(calculate(10, 5, multiply)); // 50
```

The `calculate()` function does not need to know how each operation works. It delegates that responsibility to the supplied function.

# 🗁 Practical Example: Filtering Users

Consider a user management system where different parts of an application need different user filters.

```javascript
const users = [
  { name: "Alice", age: 25, active: true },
  { name: "Bob", age: 17, active: false },
  { name: "Charlie", age: 30, active: true },
  { name: "David", age: 16, active: true },
];

function filterUsers(users, condition) {
  return users.filter(condition);
}

const activeUsers = filterUsers(users, (user) => user.active);

const adults = filterUsers(users, (user) => user.age >= 18);

console.log(activeUsers);
console.log(adults);
```

The `filterUsers()` function is reusable because the filtering condition is provided externally.

# 🗁 Function Composition

Higher-order functions can be combined to build more complex operations from smaller functions.

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

const result = numbers
  .filter((number) => number % 2 === 0)
  .map((number) => number * 10)
  .reduce((sum, number) => sum + number, 0);

console.log(result); // 120
```

The operations execute in sequence:

* `filter()` selects even numbers.
* `map()` multiplies each selected number by 10.
* `reduce()` calculates their total.

This approach is commonly called function composition or method chaining, depending on the context.

# 🗁 Higher-Order Functions and Callbacks

Callbacks and higher-order functions are closely related, but they describe different roles.

| Concept | Meaning |
| --- | --- |
| Callback | A function passed to another function |
| Higher-order function | A function that accepts or returns another function |
| First-class function | A function that can be handled like any other value |

Example:

```javascript
function execute(callback) {
  return callback();
}

function sayHello() {
  return "Hello!";
}

console.log(execute(sayHello));
```

In this example, `sayHello` is the callback, while `execute` is the higher-order function.

# 🗁 Common Use Cases

Higher-order functions are widely used in JavaScript development:

* Array transformations with `map()`, `filter()`, and `reduce()`.
* Event handling with callback functions.
* Asynchronous operations with promises.
* Middleware systems in web frameworks.
* Reusable validation and authorization logic.
* Function composition and functional programming.
* Creating configurable utilities.
* Implementing decorators and function wrappers.

# 🗁 Best Practices

* Keep callback functions focused on a single responsibility.
* Prefer reusable functions over repeating similar callback logic.
* Avoid deeply nested callbacks when simpler alternatives exist.
* Use meaningful parameter names such as `callback`, `predicate`, and `transform`.
* Prefer `map()` for transformations, `filter()` for selection, and `reduce()` for accumulation.
* Remember that `map()` and `filter()` return new arrays, while methods such as `forEach()` do not.
* Be aware that some array methods, including `sort()`, mutate the original array.
* Avoid unnecessary abstractions when a simple function is sufficient.

# 🗁 Summary

* A higher-order function accepts another function or returns a function.
* JavaScript supports higher-order functions because functions are first-class objects.
* Callbacks are commonly used as arguments to higher-order functions.
* Functions can return other functions and retain access to their enclosing scope through closures.
* Built-in methods such as `map()`, `filter()`, `reduce()`, `forEach()`, and `sort()` demonstrate higher-order function patterns.
* Higher-order functions improve code reuse, flexibility, abstraction, and composability.

# 🗁 Practice Exercises

* Create a higher-order function that accepts two numbers and a callback to perform an operation.
* Create a function that returns another function for calculating percentages.
* Implement a custom `forEach()` function that accepts an array and a callback.
* Implement a custom `map()` function that transforms every element of an array.
* Use `filter()` to select products priced above a given amount.
* Use `reduce()` to calculate the total price of a shopping cart.
* Create a reusable function that accepts a validation callback and validates user input.
* Build a function composition utility that combines two functions into one.
