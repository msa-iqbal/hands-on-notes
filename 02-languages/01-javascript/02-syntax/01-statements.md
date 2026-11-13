# JavaScript Statements

JavaScript statements are instructions that tell the JavaScript engine what actions to perform.

A JavaScript program is generally composed of multiple statements executed according to the language's execution rules.

Statements can declare variables, perform calculations, execute functions, make decisions, repeat operations, and control program flow.

Understanding statements is essential because they form the basic structure of every JavaScript program.

# 🗁 What Is a JavaScript Statement?

A statement is a unit of code that performs an action.

Example:

```javascript
const name = "Muhammad";

console.log(name);
```

This program contains two statements:

* `const name = "Muhammad";` declares a variable and initializes it.
* `console.log(name);` calls a method to display the variable's value.

Statements are the building blocks used to organize JavaScript programs.

# 🗁 Types of JavaScript Statements

JavaScript provides several categories of statements.

| Statement category | Purpose | Examples |
| --- | --- | --- |
| Declaration statements | Declare variables and functions | `let`, `const`, `var`, `function` |
| Expression statements | Evaluate expressions | `x = 10;`, `greet();` |
| Conditional statements | Execute code based on conditions | `if`, `switch` |
| Iteration statements | Repeat code | `for`, `while`, `do...while` |
| Control-flow statements | Change execution flow | `break`, `continue`, `return` |
| Exception-handling statements | Handle errors | `try`, `catch`, `throw`, `finally` |
| Module statements | Import and export modules | `import`, `export` |
| Block statements | Group multiple statements | `{ ... }` |
| Empty statements | Perform no operation | `;` |
| Debugging statements | Trigger debugging behavior | `debugger` |

# 🗁 Declaration Statements

Declaration statements introduce identifiers into the program.

**Variable declarations**

JavaScript provides three main variable declaration keywords.

```javascript
var username = "John";

let age = 25;

const country = "Bangladesh";
```

* `var` declares a function-scoped or globally scoped variable.
* `let` declares a block-scoped variable.
* `const` declares a block-scoped binding that cannot be reassigned.

**Function declarations**

A function declaration defines a reusable function.

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

# 🗁 Expression Statements

An expression statement evaluates an expression and uses its result according to the expression's behavior.

Examples:

```javascript
let number = 10;

number = 20;

console.log(number);

number++;

console.log(number);
```

Output:

```text
20
21
```

Common expression statements include:

* Assignment expressions
* Function calls
* Increment and decrement expressions
* Object and array method calls

# 🗁 Conditional Statements

Conditional statements execute different code depending on whether a condition evaluates to true or false.

**if statement**

```javascript
const age = 20;

if (age >= 18) {
    console.log("You are an adult.");
}
```

**if...else statement**

```javascript
const age = 16;

if (age >= 18) {
    console.log("You are an adult.");
} else {
    console.log("You are a minor.");
}
```

**switch statement**

```javascript
const role = "admin";

switch (role) {
    case "admin":
        console.log("Administrator access");
        break;

    case "user":
        console.log("User access");
        break;

    default:
        console.log("Unknown role");
}
```

Conditional statements help applications make decisions based on data and user input.

# 🗁 Iteration Statements

Iteration statements execute a block of code repeatedly.

JavaScript provides several loop statements.

**for loop**

```javascript
for (let i = 1; i <= 5; i++) {
    console.log(i);
}
```

**while loop**

```javascript
let count = 1;

while (count <= 5) {
    console.log(count);
    count++;
}
```

**do...while loop**

```javascript
let count = 1;

do {
    console.log(count);
    count++;
} while (count <= 5);
```

All three examples display numbers from 1 through 5.

# 🗁 Control-Flow Statements

Control-flow statements change the normal execution flow of a program.

**break**

Terminates the nearest enclosing loop or switch statement.

```javascript
for (let i = 1; i <= 10; i++) {
    if (i === 5) {
        break;
    }

    console.log(i);
}
```

Output:

```text
1
2
3
4
```

**continue**

Skips the current loop iteration and continues with the next iteration.

```javascript
for (let i = 1; i <= 5; i++) {
    if (i === 3) {
        continue;
    }

    console.log(i);
}
```

Output:

```text
1
2
4
5
```

**return**

Terminates function execution and optionally returns a value.

```javascript
function add(a, b) {
    return a + b;
}

console.log(add(10, 20));
```

Output:

```text
30
```

# 🗁 Exception-Handling Statements

Exception-handling statements allow programs to handle runtime errors.

**try...catch**

```javascript
try {
    const result = JSON.parse("{ invalid json }");

    console.log(result);
} catch (error) {
    console.log("An error occurred:", error.message);
}
```

The `try` block contains code that may throw an exception.

The `catch` block handles an exception if one occurs.

**throw**

The `throw` statement explicitly generates an exception.

```javascript
const age = 15;

if (age < 18) {
    throw new Error("Age must be at least 18.");
}
```

**finally**

The `finally` block executes after the `try` and `catch` processing, whether an exception occurred or not, subject to normal control-flow completion.

```javascript
try {
    console.log("Executing operation");
} catch (error) {
    console.log("Handling error");
} finally {
    console.log("Cleanup completed");
}
```

# 🗁 Block Statements

A block statement groups multiple statements inside curly braces (`{}`).

Syntax:

```javascript
{
    statement1;
    statement2;
    statement3;
}
```

Example:

```javascript
{
    const firstName = "Muhammad";
    const lastName = "Iqbal";

    console.log(firstName);
    console.log(lastName);
}
```

Output:

```text
Muhammad
Iqbal
```

Blocks are commonly used with:

* Conditional statements
* Loops
* Functions
* Classes
* Standalone block scopes

A block can also create a lexical scope for `let` and `const` declarations.

# 🗁 Empty Statements

An empty statement consists of a single semicolon (`;`).

It performs no operation.

Example:

```javascript
;
```

An empty statement can also appear where JavaScript expects a statement.

```javascript
if (true);
```

In this example, the `if` statement has an empty body.

This is usually accidental and should be avoided unless an empty statement is intentional.

# 🗁 Debugger Statement

The `debugger` statement can pause JavaScript execution when a debugging environment is available.

Example:

```javascript
const firstNumber = 10;

debugger;

const secondNumber = 20;

console.log(firstNumber + secondNumber);
```

When browser Developer Tools or another compatible debugger is active, execution pauses at `debugger`.

This allows developers to inspect variables and understand program execution.

# 🗁 JavaScript Statement Terminators

JavaScript uses semicolons to separate statements.

Example:

```javascript
const name = "Muhammad";
const age = 25;

console.log(name);
console.log(age);
```

Semicolons are commonly placed at the end of statements.

However, JavaScript also provides Automatic Semicolon Insertion (ASI), which can insert semicolons during parsing in certain situations.

Example:

```javascript
const name = "Muhammad"
const age = 25

console.log(name)
console.log(age)
```

This is valid JavaScript because automatic semicolon insertion handles the statement boundaries.

For consistency and readability, many projects use explicit semicolons.

# 🗁 Statement vs Expression

Statements and expressions are related but have different purposes.

An **expression** produces a value.

A **statement** performs an action or controls program execution.

Examples of expressions:

```javascript
10 + 20;

username;

firstName + " " + lastName;

age >= 18;
```

Examples of statements:

```javascript
const age = 25;

if (age >= 18) {
    console.log("Adult");
}

return age;
```

Consider this example:

```javascript
const result = 10 + 20;
```

* `10 + 20` is an expression because it produces `30`.
* `const result = 10 + 20;` is a declaration statement because it declares and initializes a variable.

# 🗁 Combining Statements into a Program

A JavaScript program combines different statements to perform a complete task.

Example:

```javascript
// Declare user information
const user = {
    name: "Muhammad",
    age: 25
};

// Define a function
function checkEligibility(person) {
    if (person.age >= 18) {
        return `${person.name} is eligible.`;
    }

    return `${person.name} is not eligible.`;
}

// Execute the function
const result = checkEligibility(user);

// Display the result
console.log(result);
```

Output:

```text
Muhammad is eligible.
```

This program combines:

* Object declaration
* Function declaration
* Conditional statement
* Return statements
* Variable declaration
* Function call

# 🗁 Best Practices

* Use meaningful variable and function names.
* Keep statements readable and focused.
* Use consistent semicolon conventions.
* Use blocks to organize related statements.
* Avoid unnecessary empty statements.
* Use `const` by default and `let` when reassignment is needed.
* Keep conditional and loop logic understandable.
* Handle exceptions where recovery or meaningful error reporting is possible.
* Avoid unnecessarily complicated expressions inside statements.

# 🗁 Key Takeaways

* Statements are instructions that make up JavaScript programs.
* JavaScript supports declaration, expression, conditional, iteration, and control-flow statements.
* Exception-handling statements manage errors.
* Block statements group multiple statements.
* Empty statements perform no operation.
* The `debugger` statement helps inspect program execution.
* Expressions produce values, while statements perform actions or control execution.
* Semicolons separate statements, and JavaScript can insert them automatically in certain situations.
* Combining statements allows developers to build complete application logic.
