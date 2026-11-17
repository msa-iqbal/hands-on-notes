# JavaScript Function Expressions

A **function expression** is a way to define a function as part of an expression and assign it to a variable or another value.

Unlike a function declaration, a function expression does not begin with the `function` keyword as a standalone statement. Instead, the function is created within an expression and can be stored, passed, or returned like any other value.

# 🗁 Basic Syntax

```javascript
const functionName = function () {
  // Function body
};
```

Example:

```javascript
const greet = function () {
  console.log("Hello, World!");
};

greet();
```

Output:

```text
Hello, World!
```

Here:

* `function () { ... }` creates a function expression.
* `greet` stores the resulting function.
* `greet()` invokes the function.

# 🗁 Function Expression vs Function Declaration

JavaScript provides two common ways to define traditional functions.

**Function declaration:**

```javascript
function greet() {
  console.log("Hello!");
}

greet();
```

**Function expression:**

```javascript
const greet = function () {
  console.log("Hello!");
};

greet();
```

The primary difference is how the function is defined and when it becomes available during execution.

| Feature | Function Declaration | Function Expression |
| --- | --- | --- |
| Syntax | `function greet() {}` | `const greet = function() {}` |
| Definition | Standalone declaration | Part of an expression |
| Hoisting | Function is initialized during declaration instantiation | Variable follows its declaration's hoisting rules |
| Invocation before definition | Generally possible within the same scope | Depends on the variable declaration |
| Name | Usually has a declared name | Can be anonymous or named |

# 🗁 Anonymous Function Expressions

An anonymous function expression is a function expression without an explicit function name.

```javascript
const calculate = function (a, b) {
  return a + b;
};

console.log(calculate(10, 20));
```

Output:

```text
30
```

The function itself has no explicitly written name. It is accessible through the variable `calculate`.

Anonymous function expressions are commonly used for:

* Assigning functions to variables.
* Passing functions as arguments.
* Defining callbacks.
* Creating short, specialized operations.

# 🗁 Named Function Expressions

A named function expression includes a name after the `function` keyword.

```javascript
const factorial = function calculateFactorial(number) {
  if (number <= 1) {
    return 1;
  }

  return number * calculateFactorial(number - 1);
};

console.log(factorial(5));
```

Output:

```text
120
```

Here:

* `factorial` is the variable holding the function.
* `calculateFactorial` is the function's internal name.
* The internal name can be used for recursion.

The internal function name is not available as an ordinary variable outside the function.

```javascript
const greet = function sayHello() {
  console.log("Hello!");
};

greet();       // Works
sayHello();    // ReferenceError
```

Named function expressions are useful when:

* Implementing recursive functions.
* Improving stack traces and debugging.
* Giving complex function expressions a descriptive identity.

# 🗁 Function Expressions and Hoisting

Function expressions do not receive the same hoisting behavior as function declarations.

Consider a function declaration:

```javascript
greet();

function greet() {
  console.log("Hello!");
}
```

Output:

```text
Hello!
```

The function declaration is initialized during the scope's declaration-instantiation phase, allowing it to be called before its textual definition.

Now consider a function expression assigned to `const`:

```javascript
greet();

const greet = function () {
  console.log("Hello!");
};
```

This produces a `ReferenceError` because `greet` is in the temporal dead zone until its declaration is evaluated.

With `var`:

```javascript
greet();

var greet = function () {
  console.log("Hello!");
};
```

This produces a `TypeError` because `var` is initialized with `undefined`, and JavaScript attempts to call `undefined` as a function.

**Important:** The function expression itself is not hoisted as an initialized function. Its availability depends on the variable declaration used to store it.

# 🗁 Functions as First-Class Values

JavaScript treats functions as **first-class values**. This means functions can be:

* Assigned to variables.
* Stored in objects and arrays.
* Passed as arguments.
* Returned from other functions.
* Assigned to properties.

Example: Assigning a function to a variable.

```javascript
const add = function (a, b) {
  return a + b;
};

const operation = add;

console.log(operation(5, 3));
```

Output:

```text
8
```

Example: Storing functions in an object.

```javascript
const calculator = {
  add: function (a, b) {
    return a + b;
  },

  subtract: function (a, b) {
    return a - b;
  }
};

console.log(calculator.add(10, 5));
console.log(calculator.subtract(10, 5));
```

Output:

```text
15
5
```

# 🗁 Passing Function Expressions as Arguments

A function expression can be passed directly to another function.

```javascript
function execute(callback) {
  callback();
}

execute(function () {
  console.log("Function expression executed!");
});
```

Output:

```text
Function expression executed!
```

The function expression is passed as an argument and invoked inside `execute`.

This pattern is commonly used in callback-based programming.

# 🗁 Returning Function Expressions

A function can return another function expression.

```javascript
function createMultiplier(multiplier) {
  return function (number) {
    return number * multiplier;
  };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5));
console.log(triple(5));
```

Output:

```text
10
15
```

The returned functions retain access to variables from their surrounding lexical scope. This behavior is closely related to **closures**.

# 🗁 Immediately Invoked Function Expressions (IIFE)

A function expression can be invoked immediately after it is created.

```javascript
(function () {
  console.log("Executed immediately!");
})();
```

Output:

```text
Executed immediately!
```

This pattern is called an **Immediately Invoked Function Expression (IIFE)**.

IIFEs were traditionally used to create private scopes and avoid polluting the global namespace.

They are covered in detail in the dedicated IIFE topic.

# 🗁 Function Expression with Parameters and Return Values

Function expressions support parameters and return values just like function declarations.

```javascript
const calculateArea = function (width, height) {
  return width * height;
};

const area = calculateArea(10, 5);

console.log(area);
```

Output:

```text
50
```

The function accepts two parameters and returns their product.

# 🗁 Practical Example: Array Operations

Function expressions can be used with built-in array methods.

```javascript
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(function (number) {
  return number * 2;
});

const evenNumbers = numbers.filter(function (number) {
  return number % 2 === 0;
});

console.log(doubled);
console.log(evenNumbers);
```

Output:

```text
[2, 4, 6, 8, 10]
[2, 4]
```

Both `map()` and `filter()` receive function expressions as callback arguments.

# 🗁 Common Mistakes

**Missing semicolon after assignment**

```javascript
const greet = function () {
  console.log("Hello!");
}
```

This is valid JavaScript because automatic semicolon insertion may handle the statement boundary. However, using a semicolon after the assignment is a consistent style:

```javascript
const greet = function () {
  console.log("Hello!");
};
```

**Calling a function expression before initialization**

```javascript
greet();

const greet = function () {
  console.log("Hello!");
};
```

This throws a `ReferenceError` because `greet` is accessed while it is in the temporal dead zone.

**Confusing a function reference with a function call**

```javascript
const greet = function () {
  console.log("Hello!");
};

const first = greet;
const second = greet();

console.log(first);
console.log(second);
```

* `first` receives the function itself.
* `second` receives the function's return value, which is `undefined` because the function has no explicit return statement.

**Trying to access a named function expression's internal name externally**

```javascript
const greet = function sayHello() {
  console.log("Hello!");
};

sayHello(); // ReferenceError
```

Use the external variable name to invoke the function:

```javascript
greet();
```

# 🗁 Best Practices

* Use `const` when assigning a function expression if the variable does not need reassignment.
* Give functions meaningful names when they improve readability and debugging.
* Use named function expressions for recursive logic when an internal function name is useful.
* Keep function responsibilities focused and predictable.
* Prefer function declarations when a standalone, hoisted function is appropriate.
* Use function expressions when functions need to be assigned, passed, returned, or created dynamically.
* Avoid unnecessary nesting that makes code difficult to understand.
* Choose arrow functions when their lexical `this` behavior and concise syntax are appropriate; they are covered separately.

# 🗁 Key Takeaways

* A function expression creates a function as part of an expression.
* Function expressions can be anonymous or named.
* They are commonly assigned to variables.
* They can be passed as arguments and returned from other functions.
* They are not hoisted as initialized function declarations.
* Their availability depends on the variable declaration used to store them.
* Function expressions are an important foundation for callbacks, higher-order functions, closures, and asynchronous programming.
