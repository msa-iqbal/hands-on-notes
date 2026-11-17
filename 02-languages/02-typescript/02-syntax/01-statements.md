# TypeScript Statements

A statement is an instruction that tells a program to perform an action.

In TypeScript, statements are used to declare variables, execute expressions, control program flow, define functions, create classes, and perform other operations.

TypeScript supports JavaScript statements and adds TypeScript-specific syntax for static type checking.

Example:

```typescript
const username: string = "John";

console.log(username);
```

In this example:

* `const username: string = "John";` is a variable declaration statement.
* `console.log(username);` is an expression statement.

# 🗁 What Is a Statement?

A statement represents an instruction executed by the program.

Example:

```typescript
let age: number = 25;

age = 30;

console.log(age);
```

Each instruction performs a specific operation:

* Declares a variable.
* Assigns a new value.
* Displays the value.

Statements are the building blocks of TypeScript programs.

# 🗁 Statement vs Expression

A statement performs an action, while an expression produces a value.

**Expression example:**

```typescript
10 + 20
```

This expression evaluates to `30`.

**Statement example:**

```typescript
const result: number = 10 + 20;
```

This is a variable declaration statement containing an expression.

Another example:

```typescript
if (10 > 5) {
    console.log("10 is greater than 5");
}
```

The `if` statement controls whether its block executes.

# 🗁 Variable Declaration Statements

Variable declaration statements create variables using `let`, `const`, or `var`.

**Using `let`:**

```typescript
let username: string = "John";

username = "David";

console.log(username);
```

**Using `const`:**

```typescript
const country: string = "Bangladesh";

console.log(country);
```

**Using `var`:**

```typescript
var score: number = 100;

console.log(score);
```

In modern TypeScript, prefer `const` by default and use `let` when reassignment is necessary.

# 🗁 Expression Statements

An expression statement consists of an expression used as a complete instruction.

Example:

```typescript
let count: number = 10;

count++;

console.log(count);
```

Here:

* `count++` is an expression statement.
* `console.log(count)` is another expression statement.

Other examples:

```typescript
const firstName: string = "John";

const fullName: string = firstName + " Smith";

console.log(fullName);
```

# 🗁 Conditional Statements

Conditional statements execute code depending on whether a condition is true or false.

**if statement:**

```typescript
const age: number = 20;

if (age >= 18) {
    console.log("Adult");
}
```

**if...else statement:**

```typescript
const age: number = 16;

if (age >= 18) {
    console.log("Adult");
} else {
    console.log("Minor");
}
```

**if...else if...else statement:**

```typescript
const marks: number = 85;

if (marks >= 80) {
    console.log("Grade A");
} else if (marks >= 60) {
    console.log("Grade B");
} else {
    console.log("Grade C");
}
```

# 🗁 Switch Statement

The `switch` statement selects a block of code based on the value of an expression.

```typescript
const role: string = "admin";

switch (role) {
    case "admin":
        console.log("Administrator");
        break;

    case "user":
        console.log("Regular User");
        break;

    default:
        console.log("Unknown Role");
}
```

The `break` statement prevents execution from continuing into the next case.

# 🗁 Loop Statements

Loop statements execute a block of code repeatedly.

**for loop:**

```typescript
for (let i: number = 1; i <= 5; i++) {
    console.log(i);
}
```

**while loop:**

```typescript
let i: number = 1;

while (i <= 5) {
    console.log(i);
    i++;
}
```

**do...while loop:**

```typescript
let i: number = 1;

do {
    console.log(i);
    i++;
} while (i <= 5);
```

# 🗁 Jump Statements

Jump statements change the normal flow of execution.

**break:**

Terminates the nearest enclosing loop or switch statement.

```typescript
for (let i: number = 1; i <= 10; i++) {
    if (i === 5) {
        break;
    }

    console.log(i);
}
```

**continue:**

Skips the current loop iteration and continues with the next iteration.

```typescript
for (let i: number = 1; i <= 5; i++) {
    if (i === 3) {
        continue;
    }

    console.log(i);
}
```

**return:**

Exits a function and optionally returns a value.

```typescript
function add(a: number, b: number): number {
    return a + b;
}

const result: number = add(10, 20);

console.log(result);
```

# 🗁 Function Declaration Statements

A function declaration defines a reusable block of code.

```typescript
function greet(name: string): void {
    console.log(`Hello, ${name}!`);
}

greet("John");
```

Explanation:

* `function` declares a function.
* `greet` is the function name.
* `name: string` defines a typed parameter.
* `: void` indicates that the function does not return a value.

# 🗁 Block Statements

A block statement groups multiple statements inside curly braces `{}`.

```typescript
{
    const username: string = "John";
    const age: number = 25;

    console.log(username);
    console.log(age);
}
```

Blocks are commonly used with:

* Conditional statements.
* Loops.
* Functions.
* Classes.
* Standalone scopes.

A block also creates a lexical scope for variables declared with `let` and `const`.

# 🗁 Empty Statement

An empty statement consists of a semicolon without an instruction.

```typescript
;
```

It performs no operation.

Example:

```typescript
let count: number = 10;

if (count > 5) {
    ;
}

console.log(count);
```

Empty statements are rarely necessary in normal application code and can sometimes be accidental.

# 🗁 TypeScript-Specific Statements and Declarations

TypeScript adds syntax that describes types and organizes code.

**Interface declaration:**

```typescript
interface User {
    id: number;
    name: string;
}
```

**Type alias declaration:**

```typescript
type Status = "active" | "inactive";
```

**Enum declaration:**

```typescript
enum Direction {
    Up,
    Down,
    Left,
    Right
}
```

**Import declaration:**

```typescript
import { UserService } from "./user-service";
```

**Export declaration:**

```typescript
export const appName: string = "My Application";
```

These constructs help organize and describe TypeScript applications. Some, such as interfaces and type aliases, exist only at compile time and are erased from generated JavaScript.

# 🗁 Type Annotations in Statements

Type annotations specify the expected type of a variable, parameter, or return value.

Example:

```typescript
let username: string = "John";

let age: number = 25;

function multiply(a: number, b: number): number {
    return a * b;
}
```

Type annotations help TypeScript detect incompatible values.

```typescript
let age: number = 25;

age = "Twenty-five";
```

The compiler reports an error because a string cannot be assigned to a number variable.

# 🗁 Statement Termination

Most TypeScript statements can be terminated with a semicolon.

```typescript
const username: string = "John";
const age: number = 25;

console.log(username);
console.log(age);
```

JavaScript and TypeScript also support automatic semicolon insertion (ASI).

Therefore, semicolons are not mandatory in every situation.

However, using a consistent semicolon style helps maintain readable code and avoid certain automatic semicolon insertion pitfalls.

# 🗁 Best Practices

* Keep statements simple and readable.
* Use meaningful variable and function names.
* Prefer `const` unless reassignment is required.
* Use explicit type annotations when they improve clarity.
* Use appropriate control-flow statements.
* Avoid deeply nested conditional blocks when simpler structures are possible.
* Use braces for conditional and loop bodies, even when they contain a single statement.
* Keep one logical instruction per line when practical.
* Follow a consistent semicolon and formatting convention.
* Avoid unnecessary empty statements.

# 🗁 Summary

Statements are instructions that define what a TypeScript program does.

You have learned about:

* Variable declaration statements.
* Expression statements.
* Conditional statements.
* Switch statements.
* Loop statements.
* Jump statements.
* Function declarations.
* Block statements.
* Empty statements.
* TypeScript-specific declarations.
* Type annotations.
* Statement termination.

Understanding statements is essential before moving into more advanced TypeScript syntax and programming concepts.
