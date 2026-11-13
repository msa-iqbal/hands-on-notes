# First JavaScript Program

Writing your first JavaScript program is the starting point for understanding how JavaScript code is written, executed, and interpreted by a JavaScript engine.

JavaScript programs can run in web browsers or outside browsers using runtimes such as Node.js.

This guide introduces the basic structure of a JavaScript program, console output, statements, expressions, comments, and common execution methods.

# 🗁 Your First JavaScript Code

Let's begin with a simple JavaScript program.

Create a file named `app.js` and write the following code:

```javascript
console.log("Hello, World!");
```

Run the program using Node.js:

```bash
node app.js
```

Output:

```text
Hello, World!
```

Congratulations! You have written and executed your first JavaScript program.

# 🗁 Understanding the Program

Let's understand each part of the code.

```javascript
console.log("Hello, World!");
```

| Part | Explanation |
| --- | --- |
| `console` | An object that provides console-related functionality |
| `.` | Property access operator |
| `log` | A method that writes information to the console |
| `()` | Function or method call parentheses |
| `"Hello, World!"` | A string value |
| `;` | Statement terminator |

The `console.log()` method is commonly used to display information and debug JavaScript programs.

# 🗁 Displaying Text

You can display different messages using `console.log()`.

```javascript
console.log("Hello!");
console.log("Welcome to JavaScript.");
console.log("I am learning programming.");
```

Output:

```text
Hello!
Welcome to JavaScript.
I am learning programming.
```

Each call to `console.log()` produces a separate console output entry.

# 🗁 Displaying Numbers

JavaScript can also display numeric values.

```javascript
console.log(10);
console.log(25.5);
console.log(-100);
console.log(2026);
```

Output:

```text
10
25.5
-100
2026
```

Numbers do not need quotation marks.

Compare:

```javascript
console.log(100);
console.log("100");
```

Both display `100`, but their values have different types:

* `100` is a number.
* `"100"` is a string.

You will explore JavaScript data types in detail in the types section.

# 🗁 Performing Mathematical Operations

JavaScript can perform arithmetic operations directly.

```javascript
console.log(10 + 5);
console.log(10 - 5);
console.log(10 * 5);
console.log(10 / 5);
console.log(10 % 3);
```

Output:

```text
15
5
50
2
1
```

Common arithmetic operators:

| Operator | Operation | Example | Result |
| --- | --- | --- | --- |
| `+` | Addition | `10 + 5` | `15` |
| `-` | Subtraction | `10 - 5` | `5` |
| `*` | Multiplication | `10 * 5` | `50` |
| `/` | Division | `10 / 5` | `2` |
| `%` | Remainder | `10 % 3` | `1` |
| `**` | Exponentiation | `2 ** 3` | `8` |

# 🗁 Working with Variables

Variables allow you to store values and reuse them throughout a program.

JavaScript provides three variable declaration keywords:

* `var`
* `let`
* `const`

Example:

```javascript
let name = "Muhammad";
let age = 25;

console.log(name);
console.log(age);
```

Output:

```text
Muhammad
25
```

You can also use variables in expressions:

```javascript
const firstNumber = 10;
const secondNumber = 20;

const result = firstNumber + secondNumber;

console.log(result);
```

Output:

```text
30
```

Use `const` when a variable binding should not be reassigned and `let` when reassignment is necessary.

# 🗁 Working with Functions

Functions allow you to organize reusable blocks of JavaScript code.

Example:

```javascript
function greet() {
    console.log("Hello, JavaScript!");
}

greet();
```

Output:

```text
Hello, JavaScript!
```

Explanation:

* `function` declares a function.
* `greet` is the function name.
* `{}` contains the function body.
* `greet()` calls the function and executes its body.

A function does not execute simply because it has been declared. It must be called unless it is invoked automatically by another mechanism.

# 🗁 Working with Parameters

Functions can receive information through parameters.

```javascript
function greet(name) {
    console.log(`Hello, ${name}!`);
}

greet("Muhammad");
greet("Ahmed");
```

Output:

```text
Hello, Muhammad!
Hello, Ahmed!
```

Here:

* `name` is a parameter.
* `"Muhammad"` and `"Ahmed"` are arguments.
* The function uses the supplied argument to produce different output.

# 🗁 Returning Values

A function can calculate and return a value.

```javascript
function add(a, b) {
    return a + b;
}

const result = add(10, 20);

console.log(result);
```

Output:

```text
30
```

The `return` statement sends a value back to the code that called the function.

# 🗁 Working with Conditions

JavaScript can make decisions using conditional statements.

Example:

```javascript
const age = 20;

if (age >= 18) {
    console.log("You are an adult.");
} else {
    console.log("You are not an adult.");
}
```

Output:

```text
You are an adult.
```

The `if` statement evaluates a condition.

* If the condition is `true`, the first block executes.
* Otherwise, the `else` block executes.

# 🗁 Running JavaScript in the Browser

JavaScript can execute directly inside an HTML document.

Create an `index.html` file:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>First JavaScript Program</title>
</head>
<body>

    <h1>My First JavaScript Program</h1>

    <script>
        console.log("Hello from JavaScript!");
    </script>

</body>
</html>
```

Open the file in a browser.

Open Developer Tools and navigate to the Console tab.

You should see:

```text
Hello from JavaScript!
```

The `script` element allows JavaScript code to be included in an HTML document.

# 🗁 Using an External JavaScript File

For larger applications, JavaScript code should generally be stored in separate `.js` files.

Project structure:

```text
javascript-practice/
├── index.html
└── app.js
```

`app.js`:

```javascript
console.log("Hello from an external JavaScript file!");
```

`index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>External JavaScript</title>
</head>
<body>

    <h1>External JavaScript Example</h1>

    <script src="app.js"></script>
</body>
</html>
```

The `src` attribute specifies the location of the external JavaScript file.

When the browser processes the script element, it loads and executes the referenced JavaScript file.

# 🗁 Understanding JavaScript Output Methods

JavaScript provides several console methods for displaying information.

| Method | Purpose |
| --- | --- |
| `console.log()` | General output |
| `console.info()` | Informational messages |
| `console.warn()` | Warning messages |
| `console.error()` | Error messages |
| `console.table()` | Displays tabular data |
| `console.debug()` | Debug-level output |

Example:

```javascript
console.log("Normal message");
console.info("Information message");
console.warn("Warning message");
console.error("Error message");

console.table([
    { name: "John", age: 25 },
    { name: "Sarah", age: 30 }
]);
```

These methods are useful when inspecting application behavior and debugging code.

# 🗁 Understanding JavaScript Execution

When you execute a JavaScript program, a JavaScript engine processes the source code.

At a high level, execution involves:

* Reading the JavaScript source code.
* Parsing the source into an internal representation.
* Performing required compilation and optimization.
* Executing the program.
* Producing results and interacting with the runtime environment.

Modern JavaScript engines use interpretation and Just-In-Time (JIT) compilation techniques to execute and optimize code.

The exact internal execution process depends on the JavaScript engine.

# 🗁 Common Beginner Mistakes

**Forgetting quotation marks around strings**

Incorrect:

```javascript
console.log(Hello);
```

Correct:

```javascript
console.log("Hello");
```

Without quotation marks, JavaScript interprets `Hello` as an identifier rather than a string literal.

**Using an incorrect file path**

Incorrect:

```html
<script src="javascript.js"></script>
```

If the actual file is named `app.js`, the browser cannot load `javascript.js`.

Correct:

```html
<script src="app.js"></script>
```

**Forgetting to call a function**

```javascript
function greet() {
    console.log("Hello!");
}
```

Declaring the function does not execute it.

Call it using:

```javascript
greet();
```

**Using an undefined variable**

```javascript
console.log(username);
```

If `username` has not been declared or otherwise defined in the accessible scope, JavaScript throws a `ReferenceError`.

Correct:

```javascript
const username = "John";

console.log(username);
```

**Running Node.js commands in the wrong directory**

If your JavaScript file is inside another directory, navigate to that directory or provide the correct file path.

```bash
node path/to/app.js
```

# 🗁 Practice Exercises

Complete these exercises to reinforce the concepts.

**Exercise: Basic output**

Write a program that displays:

```text
My name is Muhammad.
I am learning JavaScript.
JavaScript is interesting.
```

**Exercise: Arithmetic operations**

Create two variables:

```javascript
const a = 25;
const b = 10;
```

Display their:

* Sum
* Difference
* Product
* Quotient
* Remainder

**Exercise: Function**

Create a function named `multiply` that accepts two parameters and returns their product.

Call it with `5` and `10`, then display the result.

**Exercise: Conditional statement**

Create a variable named `age`.

Write a condition that checks whether the age is at least 18 and displays an appropriate message.

**Exercise: Personal introduction**

Write a JavaScript program that stores your name, profession, and country in variables and displays a short introduction.

# 🗁 Key Takeaways

* JavaScript programs are commonly stored in `.js` files.
* `console.log()` displays information in the console.
* Variables store values for later use.
* Functions organize reusable code.
* Parameters allow functions to receive input.
* The `return` statement sends values back from functions.
* Conditional statements allow programs to make decisions.
* JavaScript can execute in browsers and Node.js.
* External JavaScript files help separate application logic from HTML.
* JavaScript engines process and execute JavaScript source code.
