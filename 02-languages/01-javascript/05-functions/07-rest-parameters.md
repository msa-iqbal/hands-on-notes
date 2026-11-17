# JavaScript Rest Parameters

**Rest parameters** allow a JavaScript function to accept an arbitrary number of arguments and collect them into a single array.

They were introduced in **ECMAScript 2015 (ES6)** and provide a modern, readable alternative to using the `arguments` object.

Rest parameters use the three-dot syntax (`...`).

They are especially useful when the number of arguments is unknown or variable.

# 🗁 Basic Syntax

```javascript
function functionName(...parameterName) {
  // Function body
}
```

Example:

```javascript
function sum(...numbers) {
  console.log(numbers);
}

sum(10, 20, 30, 40);
```

Output:

```text
[10, 20, 30, 40]
```

Here:

* `...numbers` is a rest parameter.
* It collects all supplied arguments into an array.
* `numbers` is a real JavaScript array.

# 🗁 How Rest Parameters Work

Rest parameters collect arguments that are not assigned to earlier declared parameters.

```javascript
function showValues(...values) {
  console.log(values);
}

showValues("JavaScript", "TypeScript", "Python");
```

Output:

```text
["JavaScript", "TypeScript", "Python"]
```

If no arguments are supplied, the rest parameter receives an empty array.

```javascript
function showValues(...values) {
  console.log(values);
}

showValues();
```

Output:

```text
[]
```

# 🗁 Rest Parameters with Multiple Parameters

A function can combine regular parameters with a rest parameter.

```javascript
function introduce(name, age, ...skills) {
  console.log("Name:", name);
  console.log("Age:", age);
  console.log("Skills:", skills);
}

introduce("Alex", 25, "JavaScript", "React", "Node.js");
```

Output:

```text
Name: Alex
Age: 25
Skills: ["JavaScript", "React", "Node.js"]
```

In this example:

* `name` receives the first argument.
* `age` receives the second argument.
* `skills` collects all remaining arguments.

# 🗁 Rest Parameter Must Be Last

A rest parameter must always be the final parameter in a function definition.

**Correct:**

```javascript
function calculate(a, b, ...numbers) {
  console.log(a);
  console.log(b);
  console.log(numbers);
}
```

**Incorrect:**

```javascript
function calculate(...numbers, last) {
  // SyntaxError
}
```

JavaScript does not allow parameters after a rest parameter because the rest parameter collects all remaining arguments.

# 🗁 Rest Parameters with Arrow Functions

Arrow functions support rest parameters.

This is particularly useful because arrow functions do not have their own `arguments` object.

```javascript
const showValues = (...values) => {
  console.log(values);
};

showValues(10, 20, 30);
```

Output:

```text
[10, 20, 30]
```

Example with a calculation:

```javascript
const sum = (...numbers) => {
  return numbers.reduce((total, number) => total + number, 0);
};

console.log(sum(10, 20, 30, 40));
```

Output:

```text
100
```

# 🗁 Rest Parameters with Array Methods

Because rest parameters produce real arrays, you can directly use array methods such as:

* `map()`
* `filter()`
* `reduce()`
* `find()`
* `some()`
* `every()`
* `sort()`

Example:

```javascript
function getEvenNumbers(...numbers) {
  return numbers.filter(number => number % 2 === 0);
}

console.log(getEvenNumbers(1, 2, 3, 4, 5, 6));
```

Output:

```text
[2, 4, 6]
```

# 🗁 Rest Parameters with Destructuring

You can use rest parameters together with array destructuring.

```javascript
const [first, second, ...remaining] = [
  "HTML",
  "CSS",
  "JavaScript",
  "TypeScript",
  "React"
];

console.log(first);
console.log(second);
console.log(remaining);
```

Output:

```text
HTML
CSS
["JavaScript", "TypeScript", "React"]
```

Here, `...remaining` collects all array elements that were not assigned to the earlier variables.

You can also use rest properties with object destructuring:

```javascript
const user = {
  name: "Alex",
  age: 25,
  role: "Developer",
  country: "Bangladesh"
};

const { name, age, ...otherDetails } = user;

console.log(name);
console.log(age);
console.log(otherDetails);
```

Output:

```text
Alex
25
{
  role: "Developer",
  country: "Bangladesh"
}
```

Although the syntax is similar, rest parameters collect function arguments, while rest elements and rest properties collect remaining elements or properties during destructuring.

# 🗁 Rest Parameters vs Arguments Object

Before rest parameters were introduced, developers commonly used the `arguments` object to access all supplied arguments.

**Using `arguments`:**

```javascript
function sum() {
  const numbers = Array.from(arguments);

  return numbers.reduce((total, number) => total + number, 0);
}

console.log(sum(10, 20, 30));
```

**Using rest parameters:**

```javascript
function sum(...numbers) {
  return numbers.reduce((total, number) => total + number, 0);
}

console.log(sum(10, 20, 30));
```

Both produce:

```text
60
```

Comparison:

| Feature | Rest Parameters | Arguments Object |
| --- | --- | --- |
| Syntax | `...args` | `arguments` |
| Type | Real array | Array-like object |
| Array methods | Directly supported | Requires conversion |
| Arrow functions | Supported | No own `arguments` |
| Selective collection | Yes | No |
| Modern usage | Preferred | Mostly legacy or specialized usage |

Rest parameters are generally the better choice for collecting function arguments.

# 🗁 Rest Parameters vs Spread Syntax

Rest parameters and spread syntax both use three dots (`...`), but their purposes are different.

**Rest parameters collect values:**

```javascript
function sum(...numbers) {
  return numbers.reduce((total, number) => total + number, 0);
}

console.log(sum(10, 20, 30));
```

**Spread syntax expands values:**

```javascript
const numbers = [10, 20, 30];

console.log(Math.max(...numbers));
```

Output:

```text
30
```

Comparison:

| Feature | Rest Parameters | Spread Syntax |
| --- | --- | --- |
| Purpose | Collect values | Expand values |
| Usage | Function parameter definitions | Function calls, arrays, objects, and other expressions |
| Result | Array | Depends on the expression context |
| Example | `function fn(...args) {}` | `fn(...array)` |

A simple way to remember:

* Rest gathers multiple values into one array.
* Spread expands an iterable or object into individual values or properties.

# 🗁 Practical Example: Calculate Total

Rest parameters are useful for calculations involving an unknown number of values.

```javascript
function calculateTotal(...prices) {
  return prices.reduce((total, price) => total + price, 0);
}

console.log(calculateTotal(100, 200, 300));
console.log(calculateTotal(50, 75));
```

Output:

```text
600
125
```

# 🗁 Practical Example: Find the Maximum Number

```javascript
function findMaximum(...numbers) {
  if (numbers.length === 0) {
    return undefined;
  }

  return Math.max(...numbers);
}

console.log(findMaximum(10, 50, 30, 90, 20));
console.log(findMaximum());
```

Output:

```text
90
undefined
```

The function collects all arguments and expands the resulting array into `Math.max()`.

# 🗁 Practical Example: Build a Logging Utility

Rest parameters are useful when creating reusable logging functions.

```javascript
function logger(level, ...messages) {
  console.log(`[${level}]`, ...messages);
}

logger("INFO", "Application started");
logger("ERROR", "Database connection failed", "Connection timeout");
logger("DEBUG", "User ID:", 101);
```

Output:

```text
[INFO] Application started
[ERROR] Database connection failed Connection timeout
[DEBUG] User ID: 101
```

The `level` parameter receives the first argument, while `messages` collects everything else.

# 🗁 Practical Example: Create a User Profile

```javascript
function createUser(name, email, ...roles) {
  return {
    name,
    email,
    roles
  };
}

const user = createUser(
  "Alex",
  "alex@example.com",
  "Developer",
  "Admin",
  "Maintainer"
);

console.log(user);
```

Output:

```javascript
{
  name: "Alex",
  email: "alex@example.com",
  roles: ["Developer", "Admin", "Maintainer"]
}
```

This pattern is useful when a function has a fixed set of required inputs followed by a variable number of optional values.

# 🗁 Common Mistakes

**Placing the rest parameter before another parameter**

```javascript
function example(...numbers, name) {
  // SyntaxError
}
```

The rest parameter must be last.

**Treating a rest parameter as a single argument**

```javascript
function showValues(...values) {
  console.log(values.length);
}

showValues([10, 20, 30]);
```

Output:

```text
1
```

The function receives one argument: an array. The rest parameter collects that single array as an element.

To pass three separate arguments:

```javascript
showValues(10, 20, 30);
```

**Using rest parameters outside parameter lists**

```javascript
const numbers = ...[10, 20, 30];
```

This is invalid syntax. Outside parameter definitions, the three-dot operator is used as spread syntax in supported expression contexts.

Correct:

```javascript
const numbers = [...[10, 20, 30]];

console.log(numbers);
```

**Assuming rest parameters include earlier arguments**

```javascript
function example(first, second, ...remaining) {
  console.log(remaining);
}

example(10, 20, 30, 40);
```

Output:

```text
[30, 40]
```

The rest parameter collects only arguments that remain after earlier parameters have been assigned.

# 🗁 Best Practices

* Use descriptive names such as `...numbers`, `...messages`, or `...options`.
* Place rest parameters after all regular parameters.
* Prefer rest parameters over manually accessing `arguments`.
* Use array methods directly on rest parameter values.
* Validate collected values when a function expects specific data types.
* Avoid collecting arguments unnecessarily when a fixed parameter list is clearer.
* Use rest parameters with arrow functions when variable argument counts are needed.
* Distinguish rest parameters from spread syntax based on their context.

# 🗁 Key Takeaways

* Rest parameters collect an arbitrary number of function arguments into an array.
* They use the `...` syntax.
* They were introduced in ES6.
* A rest parameter must be the last parameter in a function definition.
* Rest parameters produce real arrays.
* They work with traditional functions and arrow functions.
* They are a modern alternative to the `arguments` object.
* Rest parameters collect values, while spread syntax expands them.
* They are useful for flexible functions, calculations, logging utilities, and reusable APIs.
