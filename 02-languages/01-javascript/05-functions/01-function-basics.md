# JavaScript Function Basics

A **function** is a reusable block of code designed to perform a particular task.

Functions allow developers to organize code into smaller, manageable units that can be executed whenever needed.

Instead of writing the same code repeatedly, you can define a function once and call it multiple times.

Functions are one of the fundamental building blocks of JavaScript.

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

* `function` is the keyword used to declare a function.
* `greet` is the function name.
* `()` contains the function's parameters.
* `{}` contains the function body.
* `greet()` executes the function.

# 🗁 Why Functions Are Important

Functions provide several benefits:

* **Code reusability:** Write code once and execute it multiple times.
* **Modularity:** Divide large programs into smaller components.
* **Maintainability:** Make code easier to update and manage.
* **Readability:** Give meaningful names to reusable operations.
* **Abstraction:** Hide implementation details behind a function interface.
* **Testing:** Test individual units of application logic.
* **Separation of concerns:** Keep different responsibilities independent.

**Without functions**

```javascript
console.log("Hello, John!");
console.log("Hello, Alice!");
console.log("Hello, David!");
```

**With functions**

```javascript
function greet(name) {
    console.log(`Hello, ${name}!`);
}

greet("John");
greet("Alice");
greet("David");
```

**Output**

```text
Hello, John!
Hello, Alice!
Hello, David!
```

The function eliminates repeated implementation.

# 🗁 Function Declaration

A function declaration defines a named function using the `function` keyword.

**Syntax**

```javascript
function functionName(parameters) {
    // Function body
}
```

**Example**

```javascript
function calculateTotal() {
    const price = 100;
    const quantity = 3;

    console.log(price * quantity);
}

calculateTotal();
```

**Output**

```text
300
```

A function declaration creates a reusable function that can be called from other parts of the program, subject to its scope.

# 🗁 Function Invocation

Calling or invoking a function means executing its function body.

A function can be invoked by writing its name followed by parentheses.

**Example**

```javascript
function sayHello() {
    console.log("Hello!");
}

sayHello();
sayHello();
sayHello();
```

**Output**

```text
Hello!
Hello!
Hello!
```

Each function call executes the function body independently.

**Important distinction**

```javascript
function greet() {
    console.log("Hello!");
}
```

This defines the function but does not execute it.

```javascript
greet();
```

This invokes the function.

# 🗁 Function Parameters

Parameters are variables declared in a function's definition.

They receive values when the function is called.

**Example**

```javascript
function greet(name) {
    console.log(`Hello, ${name}!`);
}

greet("John");
greet("Alice");
```

**Output**

```text
Hello, John!
Hello, Alice!
```

In this example, `name` is a parameter.

Parameters allow functions to work with different input values.

**Multiple parameters**

```javascript
function add(a, b) {
    console.log(a + b);
}

add(10, 20);
add(5, 15);
```

**Output**

```text
30
20
```

A function can accept zero, one, or multiple parameters.

# 🗁 Function Arguments

Arguments are the actual values passed to a function when it is called.

**Example**

```javascript
function multiply(a, b) {
    console.log(a * b);
}

multiply(5, 4);
```

Here:

* `a` and `b` are parameters.
* `5` and `4` are arguments.

**Parameters vs Arguments**

| Parameters | Arguments |
| --- | --- |
| Defined in function declaration | Supplied during function invocation |
| Represent input variables | Represent actual input values |
| Exist in the function definition | Exist in the function call |
| Example: `function add(a, b)` | Example: `add(10, 20)` |

# 🗁 Return Statement

The `return` statement sends a value back to the location where the function was called.

It also terminates the current function execution.

**Example**

```javascript
function add(a, b) {
    return a + b;
}

const result = add(10, 20);

console.log(result);
```

**Output**

```text
30
```

The function calculates the sum and returns the result.

The returned value can be:

* Stored in a variable.
* Used in an expression.
* Passed to another function.
* Returned from another function.

**Using the returned value in an expression**

```javascript
function square(number) {
    return number * number;
}

console.log(square(5) + square(3));
```

**Output**

```text
34
```

**Returning an object**

```javascript
function createUser(name, age) {
    return {
        name,
        age
    };
}

const user = createUser("John", 25);

console.log(user);
```

**Output**

```javascript
{
    name: "John",
    age: 25
}
```

# 🗁 Return vs console.log()

`return` and `console.log()` serve different purposes.

* `return` sends a value back to the caller.
* `console.log()` displays information in the console.

**Using console.log()**

```javascript
function add(a, b) {
    console.log(a + b);
}

const result = add(10, 20);

console.log(result);
```

**Output**

```text
30
undefined
```

The function displays `30`, but it does not return a value.

**Using return**

```javascript
function add(a, b) {
    return a + b;
}

const result = add(10, 20);

console.log(result);
```

**Output**

```text
30
```

The second function returns the result, allowing the caller to reuse it.

# 🗁 Functions Without a Return Statement

If a function does not explicitly return a value, JavaScript returns `undefined`.

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

An explicit `return;` also returns `undefined`.

```javascript
function stopExecution() {
    return;
}

console.log(stopExecution()); // undefined
```

# 🗁 Early Return

A function can return before reaching the end of its body.

This is called an **early return**.

It is commonly used for validation, guard clauses, and handling exceptional conditions.

**Example**

```javascript
function calculateDiscount(price) {
    if (price <= 0) {
        return 0;
    }

    return price * 0.10;
}

console.log(calculateDiscount(100)); // 10
console.log(calculateDiscount(-50)); // 0
```

Early returns can reduce unnecessary nesting and improve readability.

**Multiple guard clauses**

```javascript
function registerUser(name, email) {
    if (!name) {
        return "Name is required";
    }

    if (!email) {
        return "Email is required";
    }

    return "User registered successfully";
}

console.log(registerUser("John", "john@example.com"));
```

# 🗁 Function Scope

Variables declared inside a function using `let` or `const` belong to that function's lexical scope.

They cannot normally be accessed directly from outside the function.

**Example**

```javascript
function greet() {
    const message = "Hello, World!";

    console.log(message);
}

greet();

console.log(message); // ReferenceError
```

The variable `message` is local to the function.

**Global variables**

A function can access variables declared in an accessible outer scope.

```javascript
const language = "JavaScript";

function showLanguage() {
    console.log(language);
}

showLanguage();
```

**Output**

```text
JavaScript
```

A function can read accessible outer variables, but assigning to them can introduce shared mutable state.

# 🗁 Local and Global Variables

**Global variable**

A global variable is declared in the global scope and can be accessed by code that has access to that scope.

```javascript
const appName = "My Application";

function showAppName() {
    console.log(appName);
}

showAppName();

console.log(appName);
```

**Output**

```text
My Application
My Application
```

**Local variable**

A local variable is declared inside a function.

```javascript
function calculate() {
    const result = 100 + 200;

    console.log(result);
}

calculate();

console.log(result); // ReferenceError
```

The variable `result` is not accessible outside the function.

# 🗁 Function Reusability

Functions allow the same logic to be executed with different inputs.

**Example**

```javascript
function calculateArea(width, height) {
    return width * height;
}

const area1 = calculateArea(10, 20);
const area2 = calculateArea(5, 15);
const area3 = calculateArea(8, 8);

console.log(area1); // 200
console.log(area2); // 75
console.log(area3); // 64
```

The calculation is defined once and reused with different arguments.

# 🗁 Function Composition

Functions can call other functions to build more complex operations from smaller reusable operations.

**Example**

```javascript
function add(a, b) {
    return a + b;
}

function multiply(a, b) {
    return a * b;
}

function calculate(a, b, c) {
    return multiply(add(a, b), c);
}

console.log(calculate(2, 3, 4)); // 20
```

Execution:

* `add(2, 3)` returns `5`.
* `multiply(5, 4)` returns `20`.
* `calculate()` returns `20`.

This demonstrates how functions can be combined to create higher-level behavior.

# 🗁 Nested Functions

A function can be declared inside another function.

The inner function can access variables from its enclosing function.

**Example**

```javascript
function outer() {
    const message = "Hello from outer function";

    function inner() {
        console.log(message);
    }

    inner();
}

outer();
```

**Output**

```text
Hello from outer function
```

The inner function has access to the outer function's lexical scope.

Nested functions are useful for encapsulation, helper operations, and closures.

# 🗁 Function Hoisting

Function declarations are hoisted, which means they can be called before their declaration appears in the source code.

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

JavaScript makes function declarations available within their scope before normal statement execution begins.

However, function expressions assigned to variables do not behave the same way.

```javascript
greet();

const greet = function () {
    console.log("Hello!");
};
```

This produces a `ReferenceError` because `greet` is accessed before its `const` initialization.

Function hoisting is useful to understand, but declaring functions before their use can make code easier to follow.

# 🗁 First-Class Functions

JavaScript treats functions as **first-class values**.

This means functions can be:

* Assigned to variables.
* Stored in objects and arrays.
* Passed as arguments to other functions.
* Returned from other functions.
* Used in expressions.

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

The variable `sayHello` references the same function.

**Storing functions in an array**

```javascript
function greet() {
    return "Hello";
}

function goodbye() {
    return "Goodbye";
}

const actions = [greet, goodbye];

console.log(actions[0]());
console.log(actions[1]());
```

**Output**

```text
Hello
Goodbye
```

# 🗁 Function as an Argument

A function can be passed as an argument to another function.

A function passed into another function is commonly called a callback function.

**Example**

```javascript
function greet(name) {
    return `Hello, ${name}!`;
}

function processUser(name, callback) {
    const message = callback(name);

    console.log(message);
}

processUser("John", greet);
```

**Output**

```text
Hello, John!
```

The `greet` function is passed as a value and invoked inside `processUser`.

# 🗁 Function as a Return Value

A function can return another function.

**Example**

```javascript
function createGreeter() {
    return function () {
        console.log("Hello, World!");
    };
}

const greet = createGreeter();

greet();
```

**Output**

```text
Hello, World!
```

This behavior is the foundation of closures, function factories, and several functional programming techniques.

# 🗁 Pure Functions

A pure function is a function that:

* Produces the same output for the same input.
* Does not cause observable side effects.

**Example**

```javascript
function add(a, b) {
    return a + b;
}

console.log(add(10, 20)); // 30
console.log(add(10, 20)); // 30
```

The function depends only on its arguments and does not modify external state.

**Impure function example**

```javascript
let total = 0;

function addToTotal(value) {
    total += value;

    return total;
}
```

The output depends on external mutable state, and the function modifies that state.

Pure functions are easier to test, reason about, reuse, and maintain.

# 🗁 Function Execution Flow

When a function is called, JavaScript creates an execution context for that invocation.

A simplified execution flow is:

* The function is invoked.
* An execution context is created.
* Parameters and local variables become available.
* The function body executes.
* A return value is produced, or `undefined` is returned implicitly.
* The function execution context is removed from the call stack after completion.

**Example**

```javascript
function calculate(a, b) {
    const result = a + b;

    return result;
}

const total = calculate(10, 20);

console.log(total);
```

**Output**

```text
30
```

Each function invocation has its own execution context.

# 🗁 Common Mistakes

**Forgetting to call a function**

```javascript
function greet() {
    console.log("Hello!");
}

greet; // Function reference, not invocation
```

Correct:

```javascript
greet();
```

**Forgetting the return statement**

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

**Confusing parameters and arguments**

```javascript
function greet(name) {
    console.log(name);
}

greet("John");
```

`name` is the parameter, and `"John"` is the argument.

**Expecting code after return to execute**

```javascript
function greet() {
    return "Hello";

    console.log("World");
}
```

The `console.log()` statement is unreachable because the function has already returned.

**Using console.log instead of return**

```javascript
function multiply(a, b) {
    console.log(a * b);
}

const result = multiply(5, 4);

console.log(result); // undefined
```

Return a value when the caller needs to use the calculation.

**Modifying global variables unnecessarily**

```javascript
let result = 0;

function calculate(a, b) {
    result = a + b;
}
```

A function that returns its result is generally easier to reuse.

```javascript
function calculate(a, b) {
    return a + b;
}

const result = calculate(10, 20);
```

# 🗁 Best Practices

* Give functions meaningful names that describe their responsibilities.
* Keep functions focused on a single logical task.
* Prefer reusable functions instead of duplicated code.
* Use parameters to provide input rather than relying on unnecessary global variables.
* Return values when callers need to reuse computation results.
* Prefer early returns for validation and guard clauses.
* Avoid unexpected side effects.
* Keep function responsibilities small and understandable.
* Use `const` for values that do not need reassignment.
* Document complex function behavior and important assumptions.
* Write tests for important business logic.
* Prefer pure functions when practical.
* Avoid excessive nesting.
* Follow consistent naming conventions, commonly using `camelCase`.

# 🗁 Key Takeaways

* A function is a reusable block of code designed to perform a task.
* Functions are declared using the `function` keyword.
* A function executes when it is invoked.
* Parameters define input variables, while arguments provide actual values.
* The `return` statement sends a value back to the caller and terminates function execution.
* Functions return `undefined` when no value is explicitly returned.
* Variables declared inside functions have local scope.
* Function declarations are hoisted.
* JavaScript functions are first-class values.
* Functions can be passed as arguments and returned from other functions.
* Functions can be nested, composed, and reused.
* Pure functions help create predictable and maintainable code.
