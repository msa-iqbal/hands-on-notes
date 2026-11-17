# JavaScript Function Declarations

A **function declaration** is a way to define a named function in JavaScript using the `function` keyword.

It creates a function that can be invoked by its name from anywhere within its accessible scope, including before its declaration appears in the source code because function declarations are hoisted.

Function declarations are commonly used to define reusable operations, application logic, utility functions, and business rules.

**Example**

```javascript
function greet() {
    console.log("Hello, World!");
}

greet();
```

**Output**

```text
Hello, World!
```

In this example:

* `function` is the function declaration keyword.
* `greet` is the function name.
* `()` contains the parameter list.
* `{}` contains the function body.
* `greet()` invokes the function.

# 🗁 Syntax

The general syntax of a function declaration is:

```javascript
function functionName(parameter1, parameter2) {
    // Function body

    return result;
}
```

Components:

| Component | Description |
| --- | --- |
| `function` | Declares a function |
| `functionName` | Identifies the function |
| Parameters | Variables that receive input values |
| Function body | Contains the statements executed when invoked |
| `return` | Optionally returns a value to the caller |

**Example**

```javascript
function calculateTotal(price, quantity) {
    const total = price * quantity;

    return total;
}

const result = calculateTotal(100, 3);

console.log(result);
```

**Output**

```text
300
```

# 🗁 Named Function Declarations

A function declaration normally has a name that identifies the function.

The name can be used to invoke the function from its accessible scope.

**Example**

```javascript
function calculateArea(width, height) {
    return width * height;
}

console.log(calculateArea(10, 5));
```

**Output**

```text
50
```

Meaningful function names improve code readability and make function calls easier to understand.

Common naming conventions include:

* `calculateTotal()`
* `getUser()`
* `createProduct()`
* `validateEmail()`
* `sendNotification()`
* `formatDate()`

JavaScript developers commonly use `camelCase` for function names.

# 🗁 Function Declaration Hoisting

One of the most important characteristics of function declarations is **hoisting**.

JavaScript processes function declarations before executing the statements in their scope.

As a result, a function declaration can be invoked before its position in the source code.

**Example**

```javascript
greet();

function greet() {
    console.log("Hello, World!");
}
```

**Output**

```text
Hello, World!
```

This works because the function declaration is available throughout its scope before normal statement execution begins.

**Another example**

```javascript
console.log(add(10, 20));

function add(a, b) {
    return a + b;
}
```

**Output**

```text
30
```

Function declaration hoisting applies to the declaration itself, not merely to the function name as an uninitialized variable.

# 🗁 Function Declarations vs Variable Hoisting

Function declarations and variables behave differently during hoisting.

**Function declaration**

```javascript
greet();

function greet() {
    console.log("Hello!");
}
```

The function can be invoked before its declaration.

**var declaration**

```javascript
console.log(message);

var message = "Hello!";
```

**Output**

```text
undefined
```

The `var` declaration is hoisted and initialized with `undefined`, while its assignment remains at its original execution position.

**let and const declarations**

```javascript
console.log(message);

const message = "Hello!";
```

This produces a `ReferenceError` because `message` is in its temporal dead zone before initialization.

# 🗁 Function Declarations Inside Blocks

Function declarations can appear inside blocks.

Their behavior depends on the execution environment and strict-mode semantics.

In modern JavaScript, block-level function declarations are generally block-scoped.

**Example**

```javascript
"use strict";

{
    function greet() {
        console.log("Hello!");
    }

    greet();
}

greet(); // ReferenceError
```

The function is accessible inside its block but not outside it.

This makes block-level function declarations different from function declarations directly inside a function body or at the top level of a script.

For predictable behavior, declare functions at the appropriate scope rather than relying on legacy block-function behavior.

# 🗁 Function Declarations Inside Functions

A function declaration can be defined inside another function.

The inner function is accessible within its enclosing function's scope.

**Example**

```javascript
function outer() {
    function inner() {
        console.log("Inner function");
    }

    inner();
}

outer();
```

**Output**

```text
Inner function
```

The `inner()` function is local to `outer()` and cannot be called directly from outside it.

**Using outer variables**

```javascript
function calculate() {
    const price = 100;
    const quantity = 3;

    function getTotal() {
        return price * quantity;
    }

    return getTotal();
}

console.log(calculate());
```

**Output**

```text
300
```

The inner function can access variables from its enclosing lexical scope.

# 🗁 Function Parameters

A function declaration can define parameters to receive input values.

**Single parameter**

```javascript
function greet(name) {
    return `Hello, ${name}!`;
}

console.log(greet("John"));
```

**Output**

```text
Hello, John!
```

**Multiple parameters**

```javascript
function calculate(a, b, c) {
    return a + b + c;
}

console.log(calculate(10, 20, 30));
```

**Output**

```text
60
```

**No parameters**

```javascript
function getCurrentMessage() {
    return "Welcome to JavaScript!";
}

console.log(getCurrentMessage());
```

**Output**

```text
Welcome to JavaScript!
```

Functions can define zero or more parameters.

# 🗁 Return Values

A function declaration can return a value using the `return` statement.

The returned value becomes the result of the function invocation.

**Example**

```javascript
function square(number) {
    return number * number;
}

const result = square(5);

console.log(result);
```

**Output**

```text
25
```

A function can return different types of values.

**Returning a string**

```javascript
function getName() {
    return "John";
}
```

**Returning a number**

```javascript
function getAge() {
    return 25;
}
```

**Returning an array**

```javascript
function getNumbers() {
    return [10, 20, 30];
}
```

**Returning an object**

```javascript
function createUser() {
    return {
        id: 1,
        name: "John",
        role: "Developer"
    };
}
```

JavaScript functions can return any value, including other functions.

# 🗁 Default Return Behavior

If a function does not explicitly return a value, its invocation evaluates to `undefined`.

**Example**

```javascript
function greet() {
    console.log("Hello!");
}

const result = greet();

console.log(result);
```

**Output**

```text
Hello!
undefined
```

An explicit return without an expression also returns `undefined`.

```javascript
function stop() {
    return;
}

console.log(stop()); // undefined
```

# 🗁 Recursive Function Declarations

A function declaration can invoke itself.

This technique is called **recursion**.

Recursion is useful when a problem can be divided into smaller versions of the same problem.

**Example: Factorial**

```javascript
function factorial(number) {
    if (number <= 1) {
        return 1;
    }

    return number * factorial(number - 1);
}

console.log(factorial(5));
```

**Output**

```text
120
```

Execution:

```text
factorial(5)
5 * factorial(4)
5 * 4 * factorial(3)
5 * 4 * 3 * factorial(2)
5 * 4 * 3 * 2 * factorial(1)
5 * 4 * 3 * 2 * 1
120
```

A recursive function needs a base case to stop recursion.

# 🗁 Function Declarations as First-Class Values

JavaScript functions are first-class values.

A function declared using function declaration syntax can be assigned to variables, passed as an argument, stored in an object, or returned from another function.

**Assigning a function to a variable**

```javascript
function greet() {
    return "Hello!";
}

const sayHello = greet;

console.log(sayHello());
```

**Output**

```text
Hello!
```

Notice the difference:

```javascript
const first = greet;
const second = greet();
```

* `first` receives a reference to the function.
* `second` receives the value returned by invoking the function.

**Passing a function as an argument**

```javascript
function greet(name) {
    return `Hello, ${name}!`;
}

function processUser(name, callback) {
    return callback(name);
}

console.log(processUser("John", greet));
```

**Output**

```text
Hello, John!
```

# 🗁 Function Declaration vs Function Expression

Both function declarations and function expressions define functions, but their syntax and initialization behavior differ.

**Function declaration**

```javascript
function greet() {
    console.log("Hello!");
}
```

**Function expression**

```javascript
const greet = function () {
    console.log("Hello!");
};
```

Comparison:

| Feature | Function Declaration | Function Expression |
| --- | --- | --- |
| Syntax | `function greet() {}` | `const greet = function () {}` |
| Name | Normally named | Can be named or anonymous |
| Hoisting | Fully initialized before execution | Depends on the variable declaration |
| Invocation before definition | Possible within its scope | Not possible before variable initialization |
| Assignment to variable | Not required | Assigned to a variable or other expression |
| Common use | Defining named operations | Assigning or passing function values |

**Calling before declaration**

```javascript
greet();

function greet() {
    console.log("Hello!");
}
```

This works.

**Calling before function expression initialization**

```javascript
greet();

const greet = function () {
    console.log("Hello!");
};
```

This produces a `ReferenceError`.

# 🗁 Named Function Declaration vs Named Function Expression

A named function expression has an internal function name that can be used within its own function body.

**Named function expression**

```javascript
const factorial = function calculate(number) {
    if (number <= 1) {
        return 1;
    }

    return number * calculate(number - 1);
};

console.log(factorial(5));
```

**Output**

```text
120
```

The name `calculate` is available inside the function body but is not available as an ordinary variable outside it.

```javascript
console.log(calculate); // ReferenceError
```

A function declaration, by contrast, declares its name in the surrounding scope.

# 🗁 Function Declaration vs Arrow Function

Function declarations and arrow functions both define callable functions, but they have important semantic differences.

**Function declaration**

```javascript
function greet(name) {
    return `Hello, ${name}!`;
}
```

**Arrow function**

```javascript
const greet = (name) => {
    return `Hello, ${name}!`;
};
```

Comparison:

| Feature | Function Declaration | Arrow Function |
| --- | --- | --- |
| Syntax | `function` keyword | `=>` |
| Own `this` | Yes, depending on invocation | No |
| Own `arguments` object | Yes | No |
| Constructor | Can be constructible | Cannot be used with `new` |
| Hoisting | Function declaration hoisting | Depends on variable initialization |
| Generator syntax | Supports `function*` | Does not support generator syntax |

Use function declarations when you need traditional function behavior, especially when working with methods that depend on their own `this` binding or constructor behavior.

Use arrow functions when lexical `this` and concise function syntax are appropriate.

# 🗁 Function Declarations and Strict Mode

Strict mode changes certain JavaScript behaviors and makes some errors easier to identify.

Function declarations follow strict-mode scope rules.

**Example**

```javascript
"use strict";

function calculate() {
    const result = 10 + 20;

    return result;
}

console.log(calculate());
```

**Output**

```text
30
```

Strict mode also enforces block-level scoping rules for function declarations inside blocks.

It can be enabled for an entire script or within a function body.

# 🗁 Practical Examples

**Calculate the total price**

```javascript
function calculateTotal(price, quantity) {
    return price * quantity;
}

const total = calculateTotal(250, 4);

console.log(`Total: ${total}`);
```

**Output**

```text
Total: 1000
```

**Validate an email address**

```javascript
function isValidEmail(email) {
    if (typeof email !== "string") {
        return false;
    }

    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}

console.log(isValidEmail("john@example.com")); // true
console.log(isValidEmail("invalid-email"));    // false
```

**Create a user object**

```javascript
function createUser(id, name, role) {
    return {
        id,
        name,
        role,
        active: true
    };
}

const user = createUser(1, "John", "Developer");

console.log(user);
```

**Output**

```javascript
{
    id: 1,
    name: "John",
    role: "Developer",
    active: true
}
```

**Convert Celsius to Fahrenheit**

```javascript
function celsiusToFahrenheit(celsius) {
    return (celsius * 9 / 5) + 32;
}

console.log(celsiusToFahrenheit(0));  // 32
console.log(celsiusToFahrenheit(25)); // 77
console.log(celsiusToFahrenheit(100)); // 212
```

**Find the maximum number**

```javascript
function findMaximum(numbers) {
    if (numbers.length === 0) {
        return undefined;
    }

    let maximum = numbers[0];

    for (const number of numbers) {
        if (number > maximum) {
            maximum = number;
        }
    }

    return maximum;
}

console.log(findMaximum([10, 45, 20, 80, 30]));
```

**Output**

```text
80
```

**Format a user's full name**

```javascript
function getFullName(firstName, lastName) {
    return `${firstName.trim()} ${lastName.trim()}`;
}

console.log(getFullName("John", "Doe"));
```

**Output**

```text
John Doe
```

# 🗁 Common Mistakes

**Forgetting the function name**

```javascript
function() {
    console.log("Hello!");
}
```

This is invalid as a standalone function declaration because a function declaration requires a name.

An anonymous function can be used in a function expression:

```javascript
const greet = function () {
    console.log("Hello!");
};
```

**Calling a function before a function expression is initialized**

```javascript
greet();

const greet = function () {
    console.log("Hello!");
};
```

This produces a `ReferenceError`.

**Confusing function declaration with invocation**

```javascript
function greet() {
    console.log("Hello!");
}

greet;
```

This only references the function. It does not execute it.

Correct:

```javascript
greet();
```

**Forgetting to return a value**

```javascript
function add(a, b) {
    a + b;
}

console.log(add(10, 20)); // undefined
```

Correct:

```javascript
function add(a, b) {
    return a + b;
}
```

**Using a function outside its scope**

```javascript
function outer() {
    function inner() {
        console.log("Hello!");
    }
}

inner(); // ReferenceError
```

The `inner()` declaration is local to `outer()`.

**Unreachable statements after return**

```javascript
function calculate() {
    return 100;

    console.log("This will not execute");
}
```

Statements after an executed `return` are not reached.

# 🗁 Best Practices

* Use descriptive function names that communicate intent.
* Declare functions in the scope where they are needed.
* Keep each function focused on a clear responsibility.
* Prefer explicit return values for reusable calculations.
* Use parameters instead of unnecessary global state.
* Keep function bodies small and maintainable.
* Avoid relying on hoisting when it makes code harder to understand.
* Use early returns to reduce unnecessary nesting.
* Prefer pure functions for predictable business logic.
* Use function expressions or arrow functions when their semantics better fit the use case.
* Avoid creating deeply nested functions unless encapsulation or closures are useful.
* Test important function behavior, including edge cases.

# 🗁 Key Takeaways

* A function declaration defines a named function using the `function` keyword.
* Function declarations are hoisted and can be invoked before their declaration within the same accessible scope.
* Parameters receive input values supplied through arguments.
* The `return` statement sends a value back to the caller.
* Functions without an explicit return value produce `undefined`.
* Function declarations can be nested inside other functions.
* Function declarations can be recursive.
* JavaScript functions are first-class values.
* Function declarations differ from function expressions in syntax and initialization behavior.
* Function declarations have their own `this` behavior and can be used as constructors when appropriate.
* Meaningful names, clear responsibilities, and predictable return values make functions easier to maintain.
